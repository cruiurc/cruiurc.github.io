
https://www.labno3.com/2021/08/07/setting-up-a-wireguard-vpn-on-the-raspberry-pi/

在这个项目中，详细记录如何在Raspberry Pi上设置新一代的VPN协议WireGuard VPN。

Raspberry-Pi-WireGuard-VPN-Thumbnail.jpg

与OpenVPN相比，在树莓派上使用WireGuard VPN有几个优势。

WireGuard的连接速度比OpenVPN快得多，它可以在十分之一秒内完成连接。
WireGuard的代码库也比OpenVPN的代码库小得多。这使得软件更加安全，这要归功于较小的攻击可能性和更容易处理的错误。WireGuard只使用了4000行代码，而OpenVPN使用了70000多行代码。
WireGuard唯一明显的缺点是，它确实存储了连接用户的IP地址。它这样做是为了提高连接速度。
在本教程结束时，您将在树莓派上运行一个由WireGuard支持的VPN。

设备清单
以下是我在Raspberry Pi上设置WireGuard的设备清单，点击连接可直达特别优惠购买。

建议
? 树莓派
? 高速SD卡
? 树莓派4B电源
? 以太网线或Wifi
可选
? 树莓派外壳
? USB鼠标
? USB键盘
我是在安装了最新版的Raspberry Pi OS Lite的Raspberry Pi 4上测试的这个教程。

准备树莓派安装WireGuard VPN
在本节中，做一些初步的准备工作，以确保Raspberry Pi已经准备好安装WireGuard VPN软件。

1. 首先需要做的是确保Raspberry Pi使用最新的可用包。

运行以下两个命令来实现。

sudo apt update
sudo apt full-upgrade
2. 需要安装唯一需要运行的安装脚本的包。

虽然这个软件包应该在大多数Raspbian操作系统的发行版上都可以使用，但我用下面更保险的方式。

sudo apt install curl -y
在Raspberry Pi上安装WireGuard
在本节中，使用PiVPN脚本来安装WireGuard。

PiVPN使我们在树莓派上安装WireGuard的过程变得更简单，脚本为设备设置了最佳的默认值。

启动PiVPN安装脚本
1. 运行以下命令开始安装。

curl -L https://install.pivpn.io | bash
这个命令将使用curl从他们的网站上下载PiVPN安装脚本，然后直接用管道连接到bash。可以在浏览器中直接去安装PiVPN域名来验证这个脚本的内容。

将WireGuard安装到您的Raspberry Pi上
1. 迎接你的第一个画面会让你知道这个脚本要做什么。

1-PiVPN-Welcome-Install-Screen.png

2. 首先要通过这个脚本配置的是一个静态IP地址。

这个页面解释了为什么Raspberry Pi在作为WireGuard VPN服务器运行时应该有一个静态IP地址。要继续，请按ENTER键继续。

2-PiVPN-Message-about-Static-IP-Address.png

3. 你会被问到是否已经使用DHCP预留。

使用DHCP预留可以让你的路由器为Raspberry Pi分配一个IP地址。在本指南中，我们将假设你没有使用DHCP预留，并将在Pi上设置一个静态IP地址。选择<否>选项，按ENTER键继续。

3-PiVPN-DHCP-Reservation-Message.png

4. 要为WireGuard软件设置一个静态IP地址。安装脚本将希望使用你的默认设置。

如果默认IP地址和网关是对的，可以放心地选择<是>选项。按ENTER键继续执行WireGuard设置指南。

4-Allow-PiVPN-Static-IP-address.png

5. 你会被警告，当使用这种方法时，你可能会遇到IP冲突。

解决这个问题的方法是使用DHCP预留。然而，大多数路由器应该足够聪明，所以IP冲突的可能性很小。按ENTER键继续。

5-PiVPN-Potential-IP-Conflict.png

6. 这个页面说，需要指定一个本地用户来存储WireGuard配置文件。

按ENTER键继续进入下一个页面。

6-PiVPN-Message-about-Local-users.png

7. 现在可以从可用用户列表中选择。

使用箭头键突出显示该用户，然后使用空格键选择该用户。选中后，按回车键。

7-PiVPN-Select-from-user-list.png

8. 最后，可以选择要安装的VPN软件。

由于要将WireGuard安装到Raspberry Pi上，可以按ENTER键继续。原因是PiVPN脚本默认选择WireGuard。

8-Select-WireGuard-as-VPN-for-Raspberry-Pi.png

9. 这个页面可以更改WireGuard在Raspberry Pi上使用的端口。

建议保持不变，除非有特别的原因要改变端口。按 ENTER 键确认指定的端口。

9-Choose-Wireguard-Port-for-Raspberry-Pi-VPN.png

10. 这个页面只是确认设置Raspberry Pi WireGuard VPN要使用的端口。

请注意，为了能够从家庭网络以外的地方访问您的WireGuard VPN，您将需要转发这里提到的端口。这个端口的类型是UDP。确认端口仍然正确，然后按 ENTER 键继续。

10-Confirm-WireGuard-VPN-Port.png

11. 现在可以指定要为VPN客户端使用的DNS提供商。

在我的教程中，我选择了使用Cloudflare的，因为它的速度比较快，而且他们每24小时清理一次日志。

使用箭头键来浏览此菜单。找到您要使用的DNS提供商后，按空格键。选好后，按回车键确认。

11-Select-DNS-Provider-to-route-WireGuard-through.png

12. 可以指定两种不同的方式来访问您的WireGuard VPN。

使用你的公共IP地址是最简单的选择。但是，只有在你有静态IP地址的情况下才可以使用。另一种选择是使用域名。可以按照我的动态DNS指南来设置这个选项。在本指南中，我使用了公共IP地址。一旦选好后，按ENTER键继续。

12-Choose-WireGuard-access-paths.png

13. PiVPN脚本现在将生成WireGuard所需的服务器密钥。

这里你需要做的就是再次按下ENTER键。

13-WireGuard-Generating-Server-keys.png

14. 该屏幕将提供了关于无人值守升级的概述，以及为什么应该启用它们。

按ENTER键进入下一步。

14-PiVPN-Warning-about-enabling-Unattended-Upgrades.png

15. 现在可以通过选择<是>选项启用无人值守升级。

我们强烈建议您启用这些功能，以确保Raspberry Pi定期下载安全修复程序。如果不启用这个功能，WireGuard VPN就有可能受到攻击。选择好后，按ENTER键确认。

15-PiVPN-Enabling-Unattended-Upgrades-on-Pi.png

16. 现在已经成功地将WireGuard VPN软件安装到Raspberry Pi上。

这个屏幕会让你知道你还需要为用户创建配置文件，我们将在下一节介绍。按ENTER键继续最后两步。

16-Raspberry-Pi-WireGuard-Installation-Completed.png

17. 被问及是否要在继续之前重新启动Raspberry Pi。

我建议选择<是>选项。选择重启后，按两次ENTER键即可重新启动。

17-Reboot-Pi-after-WireGuard-Installation.png

在Raspberry Pi上创建第一个WireGuard配置文件
现在已经成功地将WireGuard软件安装到树莓派上，可以为它创建一个配置文件。

为了能够创建这个配置文件，再次使用PiVPN脚本。

1. 要开始为WireGuard创建一个新的配置文件，需要运行以下命令。

sudo pivpn add
2. 需要做的就是为您正在创建的配置文件输入一个名称。

例如，我将把我的档案称为 “PiMyLifeUp”。

19-Creating-a-WireGuard-Profile-on-Raspberry-Pi.png

一旦您创建了一个配置文件，它将被存储在输出中指定的目录中。

如果你按照前面的步骤使用pi用户，你将能够在/home/pi/configs目录下找到配置文件。

你可以使用这里的配置文件来设置你的WireGuard客户端。然而，还有另一种方法，我将在下一节介绍。

为WireGuard简介生成一个QR码。
在本节中，我将向您展示如何为在Raspberry Pi上生成的WireGuard配置文件生成一个QR码。

将能够使用您的设备扫描这个二维码。这样就省去了从设备上复制配置文件的麻烦。

幸运的是，PiVPN软件自带了一个二维码生成器，我们可以使用。

1. 要为您的个人资料生成一个二维码，您需要先运行以下命令。

请确保将 “PROFILENAME “替换为您在上一节中设置的名称，在我们的例子中，这将是 “PiMyLifeUp”。在我们的例子中，这将是 “PiMyLifeUp”。

pivpn -qr PROFILENAME
20-QR-Code-Generatted-for-WireGuard-connection.png

2. 然后你可以用你的iOS或Android设备扫描这个二维码。

你可以在Google Play Store和苹果App Store上找到WireGuard应用。

扫描二维码时，会要求你输入个人资料的名称。

Scan-WireGuard-QR-Code-on-iPhone.jpg

此时，您应该已经成功地在树莓派上运行了WireGuard VPN。如果你遇到任何问题或有任何反馈，请在下面留言。
