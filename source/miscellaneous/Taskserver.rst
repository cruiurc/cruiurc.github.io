Taskwarrior跨平台部署与同步
==========================

到目前为止，折腾了两三天，到今天填坑完毕，做个记录了结。

安装
--------
基于debian12，必要的依赖都已经具备了，可以直接使用apt安装:

.. code-block:: bash

    $ sudo apt install taskd

debian12下默认安装的是最新的正式版本1.1.0（最新的开发版本是1.2.0）。

初始化
--------------
手动设置数据文件夹的位置：

.. code-block:: bash

    $ export TASKDDATA=/var/taskd
    $ sudo mkdir -p $TASKDDATA

进入整个文件夹并初始化：

.. code-block:: bash

    $ taskd init
    You must specify the 'server' variable, for example:
    taskd config server localhost:53589

    Created /var/taskd/config


下面进入源文件夹下的pki目录（如果是apt安装就在``/usr/share/taskd/pki``），修改vars中的CN变量：

.. code-block:: bash

    $ cd /usr/share/taskd/pki
    $ emacs vars

设置变量
--------
修改CN变量为服务器地址，这里填写vps的IP地址：

.. code-block:: bash

    CN=xxx.xxx.xxx.xxx


CN（公用名）的值很重要。Taskwarrior 会根据此值验证服务器名称，因此请使用类似于 的值，不要指望该示例适合您。 如果不更改此值，则客户端的唯一选择是跳过部分或全部证书验证，这是一个坏主意。ack.example.com

生成和复制证书
--------------
生成并将证书复制到数据文件夹，由于上一步之后我们还在pki文件夹，可以直接生成证书：

.. code-block:: bash

    $ ./generate
    ...

    $ cp client.cert.pem $TASKDDATA
    $ cp client.key.pem $TASKDDATA
    $ cp server.cert.pem $TASKDDATA
    $ cp server.key.pem $TASKDDATA
    $ cp server.crl.pem $TASKDDATA
    $ cp ca.cert.pem $TASKDDATA

配置服务器
----------
使用刚才复制的证书配置服务器：

.. code-block:: bash

    $ taskd config --force client.cert $TASKDDATA/client.cert.pem
    $ taskd config --force client.key $TASKDDATA/client.key.pem
    $ taskd config --force server.cert $TASKDDATA/server.cert.pem
    $ taskd config --force server.key $TASKDDATA/server.key.pem
    $ taskd config --force server.crl $TASKDDATA/server.crl.pem
    $ taskd config --force ca.cert $TASKDDATA/ca.cert.pem

其他配置：

.. code-block:: bash

    $ cd $TASKDDATA/..
    $ taskd config --force log $PWD/taskd.log
    $ taskd config --force pid.file $PWD/taskd.pid
    $ taskd config --force server localhost:53589

注意这里有一个大坑，就是``localhost``这里，如果配置成vps的公网ip，后面会出现“Cannot assign requested address”的错误，导致客户端连接不上。必须是localhost或者内网IP。

所有的配置可以在下面的命令中检查：

.. code-block:: bash

    $ taskd config


其他配置选项可以在下面的命令中查看：

.. code-block:: bash

    $ man taskdrc


启动设置
--------


需要在``/etc/systemd/system``下编写一个``taskd.service``文件，以实现自启动：

.. code-block:: bash

    emacs /etc/systemd/system/taskd.service


文件的内容如下：

.. code-block:: bash

    [Unit]
    Description=Secure server providing multi-user, multi-client access to Taskwarrior data
    Requires=network.target
    After=network.target
    Documentation=http://taskwarrior.org/docs/#taskd

    [Service]
    ExecStart=/usr/bin/taskd server --data /var/taskd
    Type=simple
    User=root
    Group=root
    WorkingDirectory=/var/taskd
    PrivateTmp=true
    InaccessibleDirectories=/home /root /boot /opt /mnt /media
    ReadOnlyDirectories=/etc /usr

    [Install]
    WantedBy=multi-user.target

需要注意上面的``User``和``Group``要填写系统用户名。之后通过命令启动程序和检查：

.. code-block:: bash

    $ systemctl daemon-reload
    $ systemctl start taskd.service
    $ systemctl status taskd.service


当程序运行正常，设置启动：

.. code-block:: bash

    $ systemctl enable taskd.service

创建组织和用户
-------------
在服务器中创建组织和用户：

.. code-block:: bash

    $ taskd add org Public
    Created organization 'Public'
    $ taskd add user 'Public' 'First Last'
    New user key: cf31f287-ee9e-43a8-843e-e8bbd5de4294
    Created user 'First Last' for organization 'Public'


创建证书和密钥
-------------
需要再次到源文件夹中为用户生成证书：

.. code-block:: bash

    $ cd /usr/share/taskd/pki
    $ ./generate.client first_last


This will generate a new key and cert, named and . It is not important that 'first\_last' was used here, just that it is something unique, and valid for use in a file name. It has no bearing on security.

客户端配置
----------
在客户端通过apt安装taskwarrior，将刚才创建的证书复制到``~/.task``文件夹，并配置客户端：

.. code-block:: bash

    $ apt install taskwarrior
    $ cp first_last.cert.pem ~/.task
    $ cp first_last.key.pem ~/.task
    $ cp ca.cert.pem ~/.task

    $ task config taskd.certificate -- ~/.task/first_last.cert.pem
    $ task config taskd.key -- ~/.task/first_last.key.pem
    $ task config taskd.ca -- ~/.task/ca.cert.pem
    $ task config taskd.server -- host.domain:53589
    $ task config taskd.credentials -- Public/First Last/cf31f287-ee9e-43a8-843e-e8bbd5de4294


这里使用的``host.domain``是vps的公网地址。

同步
-----
.. code-block:: bash

    $ task sync init
    Please confirm that you wish to upload all your pending tasks to the Task Server (yes/no) yes
Syncing with host.domain:53589

    Sync successful.  2 changes uploaded.
