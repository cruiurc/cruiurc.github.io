
https://www.labno3.com/2021/08/08/build-your-own-raspberry-pi-vpn-server/

在本教程中，详细记录了使用OpenVPN软件设置Raspberry Pi VPN服务器。

Raspberry-Pi-VPN-server.jpg

还将介绍设置各种你必须做的事情，通过设置加密密钥确保连接尽可能地安全。

这个项目需要做的操作比较多，但都是些简单的操作，配置好后应该不需要任何额外的交互。

使用Raspberry Pi是一种建立虚拟专用网络（VPN）的廉价方法，它可以在不消耗大量电力的情况下保持24小时在线。它的体积小，功能强，可以同时处理几个连接，非常适合在家里私人使用。

VPN是一个非常有用的网络工具，即使是利用公共Wi-Fi，也可以加密访问互联网，而且也可以让外网的设备通过树莓派连接处于内网的设备。例如，如果你有一个网络连接的存储服务器，你想在外面访问它，那么VPN服务器就可以非常方便的实现。

设备清单
下面是在这个树莓派VPN服务器教程中利用的所有设备清单，点击连接可直达特别优惠购买。

建议
? 树莓派
? 高速SD卡
? 树莓派4B电源
? 以太网线或Wifi
可选
? 树莓派外壳
? USB鼠标
? USB键盘
为VPN服务器做准备
在开始设置Raspberry Pi VPN服务器之前，有几件事必须去做。

首先，要使用一个干净版本的Raspbian。如果你还没有安装它，想学习如何安装，如果你是新手，我的Raspbian安装指南会给你提供些帮助。

其次，在开始设置之前，确保你确实需要一个VPN，因为它可以作为一个网关进入你的家庭网络。如果你打算使用VPN，请确保家庭网络中的所有电脑都是安全的，并且你不会在本地网络中共享任何你不希望别人访问的东西。

准备VPN服务器的IP地址
决定你是要利用静态IP地址还是动态IP地址很重要，为静态IP地址设置VPN是一个相当简单的过程，不需要额外的工作。然而，如果你想利用一个动态的IP地址，你必须使用一个动态的DNS服务。如果你选择走动态DNS服务路线，那么你应该决定是使用自己的域名还是免费域名。如果你想使用自己的域名，可以使用CloudFlare这样的服务，如果你要使用免费的子域名，那么no-ip.org这样的服务将对你有用。可以查看我的指南，了解更多关于设置Raspberry Pi动态DNS的信息。

记住你为Cloudflare或no-ip.org设置的域名，因为在后面的教程中会需要这个。

Raspberry Pi VPN的端口转发
在你开始设置Raspberry Pi之前，你需要做的第三件重要事情是为OpenVPN软件进行端口转发。需要转发的默认端口是1194。 记住设置的端口，因为在后面的教程中会需要这个端口。这个端口需要使用的协议是UDP。

安装VPN服务器
1. 设置Raspberry Pi VPN服务器是一个相当复杂的过程，需要安装软件、生成加密密钥、添加端口到防火墙、设置Pi保持静态IP地址等等。

幸运的是，有一个名为PiVPN的安装脚本，可以更简单地来设置Raspberry Pi VPN服务器，这处理了设置VPN的所有繁琐工作，并减少了犯错的可能性。


在开始之前，应该首先更改默认pi用户的密码，这是为了确保如果有人非法访问你的VPN，他们会被拒之门外。

passwd
2. 密码改好后，就可以开始在树莓派上设置VPN服务器了。运行下面的命令来开始这个过程，这个命令从PiVPN的GitHub页面下载安装脚本并运行它。

通常情况下，直接从URL中运行脚本不太好，因为它极有可能被人植入恶意程序。不过这个是验证过的可信源，如果想自己查看代码，只要到脚本的位置就可以了。

curl -L https://install.pivpn.io | bash
3. 运行了上述命令后，会看到以下屏幕。这个屏幕会有一些文字告诉你，你即将安装OpenVPN。

要进入下一个屏幕，需要按ENTER键。

1-PiVPN-Automated-Installer-Welcome-Screen.png

4. 下一个屏幕说需要为VPN设置一个静态IP地址。

这样，当树莓派重新启动时，它将尝试使用相同的IP地址。如果本地IP发生变化，可能会失去对VPN的访问。

2-PiVPN-Static-IP-Address-Required.png

5. 现在会被问到是否在路由器上使用DHCP预留。

如果你不知道什么是DHCP预留或如何使用它，请选择<否>继续。

3-PiVPN-Are-you-using-DHCP-Reservation.png

6. 在这里，选择<是>，将当前的IP地址和网关设置为静态。

如果本页显示的IP地址不对，请选择<否>。

4-Allow-PiVPN-to-set-a-Static-IP-address.png

7. 这个屏幕警告你，你的路由器有可能会把IP地址分配给其他设备。

可以使用DHCP保留来避免这个问题。不过，大多数路由器都很聪明，可以避免这个问题。要继续，选择<Ok>并按ENTER键。

5-PiVPN-warning-about-IP-Conflict.png

8. 该界面说明需要设置一个拥有OpenVPN配置文件的用户。

选择<Ok>，按ENTER键进入下一个屏幕。

6-PiVPN-Choose-local-user-to-hold-config.png

9. 将看到一个可以拥有Raspberry Pi的VPN配置文件的用户列表。

在本教程中，使用的是pi用户。如果想使用其他用户，请使用箭头键和空格键来选择它。选择好了之后，按ENTER键继续。

7-PiVPN-Choose-user-for-VPN-config.png

10. 现在将被要求选择要在Raspberry Pi上安装的VPN类型。

两种选择是WireGuard和OpenVPN。在本指南中，在Raspberry Pi上使用OpenVPN（1.）。使用箭头键和空格键来选择它。一旦选择了OpenVPN，按ENTER键继续（2.）。

8-PiVPN-Choose-software-type-for-VPN-OVPN.png

10. 现在可以决定是否要在Raspberry Pi上自定义安装OpenVPN。

PiVPN团队选择的设置对大多数用户来说是最好的。但是，如果你喜欢，你可以修改这些设置。对于本指南，我使用了默认设置。要继续，选择<否>，然后按ENTER键。

9-PiVPN-Select-easy-mode-installation.png

11. 现在选择OpenVPN运行的端口。

在本教程中，使用默认的1194端口。只有在有充分理由的情况下，才应该更改端口。一旦定义了端口，选择<Ok>并按ENTER键。

10-Set-default-OpenVPN-port.png

14. 会被要求确认为OpenVPN安装设置的端口。

如果一切正常，则选择<是>继续。

11-Confirm-OpenVPN-Port-Settings.png

15. 下一步是选择一个DNS提供商。DNS提供商是将像https://pimylifeup.com这样的URL解析成IP地址的机构。

在我的指南中，我使用了Cloudflare的DNS服务器。Cloudflare每24小时刷新他们的日志，并不跟踪查询IP地址。要选择Cloudflare或其他DNS提供商，需要使用ARROW键（1.一旦你悬停在你想要的DNS提供商上，按SPACEBAR键选择它。最终确认后，可以按ENTER键继续。

12-PiVPN-Select-DNS-Provider.png

16. 需要决定是要使用公共IP地址还是DNS名称。

如果使用的是动态IP地址，我建议使用公共DNS名称选项。如果您需要帮助，可以按照我的指南在树莓派上设置动态DNS。

由于我使用的是静态的公网IP地址，所以在本指南中我不用更改他。可以使用箭头键在选项之间进行切换。选择好之后，按空格键确认。

13-PiVPN-Select-Public-IP-Or-DNS.png

17. 下一步只是向你解释PiVPN脚本即将生成HMAC密钥和服务器密钥。

这些密钥是构成Raspberry Pi的VPN加密部分的一部分。按ENTER键继续引导。

14-PiVPN-Generating-Server-and-HMAC-keys.png

18. 现在，将看到一个关于无人值守升级的简单说明。

这个功能使得Raspberry Pi操作系统每天自动下载安全包更新。按ENTER键进入该设置的实际配置页面。

15-PiVPN-Message-about-enabling-Unattended-Upgrades.png

19. 在这个屏幕上，强烈建议选择<是>来启用无人值守升级。启用此功能将确保您的Raspberry Pi始终拥有最新的软件包。

如果将此功能关闭，可能会对您的树莓派的VPN和您的家庭网络造成重大安全风险。完成后，按回车键确认设置。

16-PiVPN-Enable-Unattended-Upgrades.png

20. 现在你已经完成了OpenVPN在Raspberry Pi上的安装。

虽然你还需要完成一些事情才能允许连接，但你现在已经完成了大约90%的设置指南。

17-PiVPN-Installation-Completed.png

21. 现在看到一个屏幕，要求重新启动Raspberry Pi。

按ENTER键选择<Yes>选项，进入以下两个屏幕。安装OpenVPN后重启树莓派是至关重要的一步。

18-Reboot-Pi-after-PiVPN-installation.png

设置第一个OpenVPN用户
1. 通常为OpenVPN设置一个用户是一个痛苦的过程，因为你必须为用户生成单独的证书，幸运的是，可以通过一个命令来完成，这要感谢PiVPN。

要开始添加用户，请运行以下命令。

sudo pivpn add
在这个屏幕上，需要为客户输入一个名称，这个名称将作为一个标识符，以便可以区分不同的客户。它还会要求你为客户端设置一个密码，重要的是要让这个密码很安全，不容易被猜到，这样才能保证加密密钥的安全。所以，如果有人能轻易猜到密码，就会严重降低你VPN的安全性。

Pivpn-add.png

一旦你按下回车键，PiVPN脚本将告诉Easy-RSA为客户端生成2048位RSA私钥，然后将文件存储到/home/pi/ovpns。/home/pi/ovpns是在接下来的几个步骤中必须获得访问权的文件夹，这们就可以将生成的文件复制到我们的设备上。确保这些文件的安全，因为它们是你访问VPN的唯一途径。

2. 现在新客户端已经用密码设置好了OpenVPN，需要把它连接到打算连接的设备上。

最简单的方法是在家庭网络中使用SFTP。在继续本教程之前，请确保已经安装了一个可以处理SFTP连接的程序，如FileZilla。开始，通过SFTP登录到Raspberry Pi。记得在Raspberry Pi的IP地址前输入sftp://。如果你没有Pi的本地地址，请在终端使用hostname -I命令。一旦您输入了IP地址、用户名和密码，请按快速连接按钮。

SFTP-Details.png

3. 成功登录后，需要寻找ovpns文件夹，因为需要的文件就在这里。

找到文件夹后，双击它。

SFTP-ovpns.png

4. 现在，需要做的就是把你想要的.ovpn文件拖到电脑上安全的地方。这个文件包含了我们需要连接到VPN的数据，所以要保证这个文件的安全。

这也是别人有可能访问你的VPN的唯一途径，所以保持密码和文件的安全是非常重要的。如果有人获得这些权限，就有可能对你的网络造成一定的危害。

SFTP-ovpns-download-1.png

5. 现在设备上有了.opvn文件，可以用它来连接到VPN。

.opvn文件存储了我们建立安全连接所需要的一切。它包含了要连接的网络地址，以及所有需要的加密数据。它唯一不包含你的密码，所以当你连接到VPN时，你需要输入这个。要使用的客户端是OpenVPN官方客户端，可以从他们的OpenVPN官方网站获得。

下载并安装此客户端，第一次运行时，会自动最小化到任务栏，右键点击图标，然后选择 “导入文件…”

OpenVPN-GUI.png

6. 会看到一个文件资源管理器的屏幕，在这里进入你之前保存.opvn文件的地方。

找到后，双击文件导入OpenVPN客户端。

Select-ovpn-file.png

7. 现在你应该看到一个对话框，告诉你文件已经成功导入OpenVPN。

只需点击 “确定 “按钮即可继续。

Ovpn-file-imported-succesfully.png

8. 再次右击任务栏中的OpenVPN客户端图标，这次点击 “连接 “按钮。

OpenVPN-GUI-2.png

9. 现在OpenVPN客户端将尝试读取位于.opvn文件中的数据。

由于我已经设置了一个密码，现在会要求你输入你在本教程中早先设置的密码。一旦确定输入了正确的口令，请点击 “确定 “按钮。

Ovpn-enter-password.png

10. OpenVPN客户端现在将尝试连接到你的树莓派的VPN服务器。如果OpenVPN图标变成了纯绿色，那么说明你已经成功连接到了VPN。

如果它变成黄色，并且在60秒后未能变成绿色，这意味着有东西导致连接失败。大多数情况下，连接失败是由端口转发问题引起的，比如我的路由器，就有很多端口转发问题。最简单的方法是谷歌你的路由器的型号，找到解决端口转发问题的方法。


有些ISP（互联网服务提供商）也会屏蔽特定的端口，所以最好检查一下你的ISP是否屏蔽了你计划使用的端口。

如果你使用的是动态DNS服务，那么请确保该服务正以你的最新IP地址正确更新，如果IP地址已经改变，但DNS设置没有改变，那么将导致连接失败。

希望到现在，你已经有了一个功能齐全的VPN，你也能够成功连接上它。

从树莓派上卸载VPN
1. 如果出于某种原因，想从树莓派中删除VPN，可以简单地利用以下命令。

该命令将利用pivpn软件卸载VPN。

sudo pivpn uninstall
我希望本教程已经向你展示了如何设置Raspberry Pi VPN服务器，你没有遇到任何问题。对于任何希望建立一个廉价的始终在线的VPN网络的人来说，这无疑是一个非常有用的项目。如果你有一些反馈、技巧或遇到任何问题想要分享，那么请不要犹豫，在下方留言。
