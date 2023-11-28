Task server
=============

作为重度linux和android用户，体会到taskwarrior在命令行中使用的简便后，便萌发了是不是可以把它同步到手机端的想法。于是，就有了这篇文章。该文章主要参考了taskwarrior官方的安装手册

环境准备
目前，基本想法是把taskwarrior server端和desktop端都放在PC上。基本配置如下：

|设备|os| |:-|:-| |笔记本|ubuntu 18.04LTS| |华为Mate10|EMUI 10|

其中，笔记本的用户名为 blum。

taskwarrior server端的安装
由于只是方便手机和电脑的同步（不考虑云端），server端就放在了笔记本电脑中。

#安装taskwarrior的服务器端
sudo apt-get install taskd
#设置taskd的数据目录
export TASKDDATA=/var/lib/taskd
#创建taskd的数据目录
sudo mkdir -p /var/lib/taskd
#创建taskd的设置文件
taskd init
#以上命令会在$TASKDDATA目录下生成config文件
在对server进行配置之前，需要先成安全相关的秘钥。

#进入taskd的pki目录
cd /usr/share/taskd/pki/
#编辑taskd的配置变量
sudo vim vars
这个文件需要有针对型的修改，其中过期日期建议修改为3650（这样10年不用修改配置了）。ORGANIZATION最好对应于后面创建的org，这里是随便写的。CN填写服务器的IP地址。这里特别需要注意：如果只是本机使用，这里可以用localhost；但是，如果想使用android访问，就必须要使用ip地址。由于我使用的网络环境比较简单（可以把wlan的ip静态配置为192.168.0.107），就设置为了固定的ip地址。剩余关于地区的信息就自行发挥就好了。

BITS=4096
EXPIRATION_DAYS=3650
ORGANIZATION="linux"
CN=192.168.0.107
COUNTRY=China
STATE="Hunan"
LOCALITY="haha"
配置变量修改后，就可以开始生成对应的秘钥文件了。

#生成pem秘钥文件
sudo ./generate
#以上命令会生成6个文件，分别为client,ca和server相关的key和ca
#这些文件是服务器用来认证用的
#需要注意的是，这里的client.*.pem文件是对所有用户都相同（无论后面创建的用户为何，这里的client.*.pem都不会变）
有了这些秘钥文件后，接下来就开始对服务器端的配置进行修改。

#把秘钥文件复制到taskd的数据目录
sudo cp /usr/share//taskd/pki/*.pem $TASKDDATA
#这里其实需要的是client和server的4个文件以及ca.cert.pem
#ca.key.pem没法发现有啥用
#接下来，要留意文件的权限
cd $TASKDDATA
sudo chown blum:blum *
taskd config --force client.cert $TASKDDATA/client.cert.pem
taskd config --force client.key $TASKDDATA/client.key.pem
taskd config --force server.cert $TASKDDATA/server.cert.pem
taskd config --force server.key $TASKDDATA/server.key.pem
taskd config --force server.crl $TASKDDATA/server.crl.pem
taskd config --force ca.cert $TASKDDATA/ca.cert.pem
#其实，以上这些命令就是对config文件进行了修改
#接下里，配置log文件和服务器地址等
taskd config --force log $PWD/taskd.log
taskd config --force pid.file $PWD/taskd.pid
taskd config --force server 192.168.0.107:53589
此时，服务器端就已经配置完毕。为了能够自动启动，我们还需要修改以下文件/etc/systemd/system/multi-user.target.wants/taskd.service。

其中，核心是留意以下行

6 [Service]
  # --data要修改为/var/lib/taskd
  7 ExecStart=/usr/bin/taskd server --data /var/lib/taskd
  # User和Group要修改为用户名
 10 User=blem
 11 Group=blem
 12 WorkingDirectory=/var/lib/taskd
最后，开启并查看服务情况。

sudo systemctl daemon-reload
sudo systemctl start  taskd.service
sudo systemctl status taskd.service
sudo systemctl enable taskd.service
如果服务启动过程中有问题，可以通过sudo systemctl status taskd.service确认其中的信息。

如果服务启动后，sync有问题，可以查看$TASKDDATA/taskd.log文件。

为了能够添加用户，支持task的同步，服务器还需要一些操作，请参考下一节。

taskwarrior 桌面端的安装
在ubuntu中，安装taskwarrior非常方便。

sudo apt-get install taskwarrior
然后，就可以进行测试了。

task add 'finish setting up taskwarrior'
接下来，开始在服务器端生成用户的秘钥。

#添加名称为linux的组织
$ taskd add org rctc
#添加用户Last First。
$ taskd add user 'rctc' 'cr'
New user key: cf31f287-ee9e-43a8-843e-e8bbd5de4294
Created user 'cr' for organization 'rctc'
这里的组织名、用户名和用户key都要记录下来。同步会需要用到这些配置信息。

cd /usr/share/taskd/pki/
#生成用户专属的秘钥文件
sudo ./generate.client cr
#复制到task目录
sudo cp /usr/share/taskd/pki/cr.*.pem ~/.task
sudo cp /usr/share/taskd/pki/ca.cert.pem  ~/.task
cd ~/.task
sudo chown blem:blem *
task config taskd.ca -- ~/.task/ca.cert.pem
task config taskd.certificate -- ~/.task/First_Last.cert.pem
task config taskd.key -- ~/.task/First_Last.key.pem
task config taskd.server -- 192.168.0.107:53589
task config taskd.credentials -- linux/First Last/cf31f287-ee9e-43a8-843e-e8bbd5de4294
已经以上配置后，~/.taskrc就会包含如下内容：

taskd.server=192.168.0.107:53589
taskd.key=\/home\/blem\/.task\/First_Last.key.pem
taskd.certificate=\/home\/blem\/.task\/First_Last.cert.pem
taskd.ca=\/home\/blem\/.task\/ca.cert.pem
taskd.credentials=linux\/First Last\/cf31f287-ee9e-43a8-843e-e8bbd5de4294
可以额外添加额外几行，尤其是41行

37 uda.totalactivetime.type=duration
 38 uda.totalactivetime.label=Total active time
 39 uda.totalactivetime.values=
 40 report.list.labels=id,start.age,entry.age,totalactivetime,...,urgency
 41 taskd.trust=ignore hostname
接下来，就可以开始同步了。

task sync init
task sync
taskwarrior android端的安装
从https://f-droid.org/en/packages/kvj.taskw/中获得taskwarrior-android.apk(Version 0.1.160410 (3))

安装后，给予应该有的权限。然后，开始配置。

首先，打开软件点击左上角的三个杠，然后添加新的账号。这里账号名称随意。然后，点击最下角的配置。输入下列信息：

taskd.certificate=First_Last.cert.pem
taskd.key=First_Last.key.pem
taskd.ca=ca.cert.pem
taskd.server=192.168.0.107:53589
taskd.credentials=linux\/First Last\/cf31f287-ee9e-43a8-843e-e8bbd5de4294
taskd.ciphers=NORMAL
android.sync.periodical=60
android.sync.onchange=60
android.sync.onerror=30
android.debug=y
其中，最后几行用来设置同步的间隔（单位为分钟），最后一行用来调试。

然后，把手机插入电脑，把之前配置客户端的3个pem文件复制到/sdcard/Android/data/kvj.taskw/files/00000-data-dir-name000/类似的文件中。具体可以在手机存储内容中搜索kvj.taskw目录获得。

然后，就可以点击云按钮进行同步啦！

最后
如果上述情况中出现无法同步的情况，希望你重头多来2次，肯定就可以成功了。

编辑于 2020-03-04 21:41


cr
ebef091d-053d-4147-9ed4-34294bc300b5

27fdc793-9af0-467e-bdbe-daf847de3d2a
