---
type: article
tags: termux debian
title: Termux环境安装使用Debian
---

# Termux环境安装使用Debian

## 安装系统

termux安装proot-distro

```
pkg install proot-distro
```

查看可安装的版本

``` 
proot-distro list
```

安装debian

```
proot-distro install debian
```

使用anlinux安装Xfce4桌面

```
wget https://raw.githubusercontent.com/EXALAB/AnLinux-Resources/master/Scripts/DesktopEnvironment/Apt/Xfce4/de-apt-xfce4.sh && bash de-apt-xfce4.sh
```

进入debian

```
proot-distro login debian
```

## 修改vncserver屏幕分辨率

```bash
vi /usr/local/bin/vncserver-start
```

开启vncserver

```
vncserver-start
```

关闭vncserver

```
vncserver-stop
```

vncserver默认端口号为1

安装火狐浏览器(只有debian行)

```
apt install firefox-esr
```

安装idea,pycharm直接使用firefox进官网下载tar.gz压缩包,解压后即可使用



## 与debian进行ssh连接



开启ssh服务

```bash
service ssh start
```

查看ssh状态

```bash
service ssh status
```

提示如下错误

```bash
sshd is not running ... failed!
```

由于termux默认使用ssh端口都在8022,termux中Debian使用ssh自然不能用22端口,思考后尝试修改ssh的端口号为8088

```bash
vi /etc/ssh/sshd_config  #在Debian中修改ssh配置
```

```bash
Port 8088 #修改端口
PermitRootLogin yes #运行用root账户登录
```

**注意：/etc/ssh/ssh_config设置的是客户端设置，不要把这个配置修改了，若在此文件下修改端口，进行ssh连接时，默认远程（对方）接收端口就为你设置的端口，通常远程ssh接收端口为22，这样就造成了连接错误**

修改完后重启ssh服务

```bash
service ssh restart
```

再查看ssh状态发现开启成功,但是不能进行ftp传输

在termux下关闭ssh测试一下是否要termux也开启ssh,proot容器中debian才能使用ssh

```bash
pkill sshd
```

由于是ssh连上 termux在xshell的termux里开启的debian,termux原终端断开后自然两个都端口,这时再进入termux本机开启一下debian

开启ssh服务正常,Xshell与之正常连接,事实证明termux的ssh与termux安装proot容器内debian的ssh服务不受影响

## 文件访问

我们继续尝试开启debian的ftp服务

安装ftpd

```bash
apt install vsftpd
```

尝试启动vsftpd服务

```bash
service vsftpd start
```

自然启动失败

同ssh一样去修改端口

```bash
vi /etc/vsftpd.conf
```

添加如下内容

```bash
listen_port=8089
```

重启vsftpd服务,发现vsftpd开启成功

尝试使用XFtp连接,连接失败

在debian中输入vsftpd提示

```bash
500 OOPS: could not bind listening IPv6 socket
```

修改配置文件后又提示

```bash
500 OOPS: could not bind listening IPv4 socket
```

使用pure-ftpd也连接不上

暂且找不到使用ftp的办法

好在在频道大佬的帮助下找到了**termux中proot-distro存放debian 的路径**

```shell
/data/data/com.termux/files/usr/var/lib/proot-distro/installed-rootfs/debian
```

后面在github proot-distro仓库的讨论下提问了这个问题，被proot-distro的作者回答，好像是我pure-ftpd的打开方式不对

成功建立了ftp连接,下面是大佬回答，我确实在配置文件改了端口，不知道为什么不对。。。

>For Debian:
>
>```bash
># Install server
>apt update
>apt install pure-ftpd
>
># set password for root
>passwd
>
># start ftp server
>pure-ftpd -S 2121 -l unix
>```
>
>Connect using your device IP, port 2121, user name root and its password. Worked for me.
>
>I guess your real issue is either server was not started or you trying to use default FTP port (21) which can't work.

关于-l unix,chatgpt这样解释

>在`pure-ftpd`命令中，选项`-l`被用来指定用户认证方式。而`unix`则是一种用户认证方式，它表示使用本地Unix/Linux系统的用户账户数据库来进行FTP用户的认证。具体来说，`pure-ftpd`是一个开源的FTP服务器软件，用于提供文件传输服务。它支持多种用户认证方式，其中一种就是基于本地系统用户账户的认证。通过使用`-l unix`选项，你告诉`pure-ftpd`使用本地系统的用户账户数据库进行FTP用户的认证。这意味着只有具有有效系统账户的用户才能登录到FTP服务器，并且他们将使用其系统账户的用户名和密码进行认证。这种认证方式通常会使用系统的 `/etc/passwd` 文件来验证用户。总之，`-l unix`选项告诉`pure-ftpd`使用本地系统的用户账户数据库（通过读取`/etc/passwd`等文件）来进行FTP用户的认证。

## 环境配置文件

### termux配置

~/.bashrc

```bash
alias debian='proot-distro login debian'
alias debianHome='cd /data/data/com.termux/files/usr/var/lib/proot-distro/installed-rootfs/debian'
```

### debian配置

~/.bashrc

```bash
alias vnc='vncserver-stop;vncserver-start'
```

/etc/profile

```bash
export JAVA_HOME=~/Enviroments/jdk1.8.0_381
export MAVEN_HOME=~/Enviroments/apache-maven-3.9.4
export PATH=$PATH:$MAVEN_HOME/bin:${JAVA_HOME}/bin
```

