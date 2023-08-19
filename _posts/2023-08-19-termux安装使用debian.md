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

暂且找不到使用ftp的办法

好在在频道大佬的帮助下找到了**termux中proot-distro存放debian 的路径**

```shell
/data/data/com.termux/files/usr/var/lib/proot-distro/installed-rootfs/debian
```



