---
title: 树莓派精简系统DietPi入坑指南
date: 2017-02-03 22:46:10
tags: [raspberry pi,DietPi]

---

树莓派的官方系统是Raspbian，很稳定，[https://www.raspberrypi.org/downloads/](https://www.raspberrypi.org/downloads/)，默认不开启SSH，USB输出电压默认带不动2.5寸机械硬盘，不超频，烧写镜像到microSD卡，最好class 10及以上、sandisk牌。烧写镜像软件用[usb image tool](http://www.alexpage.de/usb-image-tool/)或者[Win32 Disk Imager](https://sourceforge.net/projects/win32diskimager/)。

## DietPi介绍
这里，推荐一个轻量级的系统，DietPi。

[http://dietpi.com/](http://dietpi.com/#noAction)

原生系统Raspbian官方推荐8GB class 4 SD card，DietPi只需要2GB的SD卡就可以启动了。

默认开启了Dropbear，一个轻量级的SSH server 支持SCP和SSH2，不带SFTP支持，需要SFTP的看下面。
[https://www.digitalocean.com/community/tutorials/how-to-configure-proftpd-to-use-sftp-instead-of-ftp](https://www.digitalocean.com/community/tutorials/how-to-configure-proftpd-to-use-sftp-instead-of-ftp)

默认是root帐户，建议增加一个用户帐户。
```Bash
useradd pi # pi = your account name
passwd xin # 回车输入用户密码
mkdir /home/
mkdir /home/pi/ # 不给用户分配目录好像ssh登陆会报错
chown pi:pi /home/pi/ #给目录权限
```

## 修改镜像为中科大镜像站

编辑`/etc/apt/sources.list`文件。用`#`注释掉原文件所有内容，用以下内容取代：
```
deb http://mirrors.ustc.edu.cn/raspbian/raspbian/ jessie main non-free contrib
deb-src http://mirrors.ustc.edu.cn/raspbian/raspbian/ jessie main non-free contrib
```
修改`/etc/apt/sources.list.d/raspi.list`为以下内容
```
deb https://mirrors.ustc.edu.cn/archive.raspberrypi.org/ jessie main ui
```
实际修改是，将树莓派基金会的网址由archive.raspberrypi.org替换为mirrors.ustc.edu.cn/archive.raspberrypi.org，不然update的时候仍然很慢。

注意，这里用到了https的源，防止电信劫持，https需要额外的一行命令支持，命令如下：

```
//使树莓派支持https
sudo apt-get install apt-transport-https 
```
编辑此文件后，请使用sudo apt-get update命令，更新软件列表。

## 常用的命令

### dietpi-software

系统的软件仓库，可以多选然后一键安装，软件列表如下：
[http://dietpi.com/phpbb/viewtopic.php?f=8&t=5#p5](http://dietpi.com/phpbb/viewtopic.php?f=8&t=5#p5)

推荐的软件配置：
>* SSH：Dropbear
>* File Servers: ProFTP
>* DownloadD Utility: aria2
>* DNS Servers: Pi-hole
>* Webserver: Lighttpd
>* Remote Access: 花生壳
>* AirPlay ios通过wifi来播放: ShairPort Sync


### dietpi-update

更新DietPi的版本

### dietpi-config

设置树莓派的选项

Display Settings: 这里设置GPU memory splits （我只用ssh登陆，选了server），是否开启摄像头，分辨率，和hdmi相关选项，我的树莓派连接1080p的电视机显示内容超出了屏幕， 设置一下`overscan = true`就好。

Audio Options: 需要用外置USB声卡的童鞋在这里修改

Performance Options：设置超频、高温保护

Security Options: 改密码和hostname

Regional / Language Options: Locale增加`zh_CN.UTF8`，TimeZone选`Asia/SHanghai`，时间日期基本就同步过来了，中文显示也没有问题。

AutoStart Options：我选的0，console界面 手动登陆。

另外，`DietPi-Ramlog`默认就好，减少系统频繁对SD卡的写入。`htop`很直观地看到系统运行情况。`cpu`能看到cpu的温度和性能情况 `treesize`能读取目录的大小。以上都是raspbian默认不具备但DietPi默认开启的。

dietpi-update会覆盖一遍内核，所以想用`sudo BRANCH=next rpi-update`升级内核到4.9+的童鞋还是等等吧，我也想开启google bbr 加速树莓派的网络传输速度ORZ。

总之，用了DietPi，感觉腿也有劲了腰也不酸了，常年内存使用不超过150MB。

## 外网访问树莓派
用了花生壳的ddns，教程如下：

<http://hsk.oray.com/news/2688.html>

需注意的是，如果路由器的上游是电信光猫，那ddns的网址只能获取光猫的登陆页面，因为光猫本身也是一个限制很多的路由，外网访问不到内网二级路由下面的树莓派。所以买了TP-LINK TL-EP110 EPON终端，光纤输入，千兆网口输出。然后路由器端用PPPOE拨号连接宽带，然后路由器的所有端口转发到树莓派的ip，才能实现外网访问树莓派。

也就是说，外网访问树莓派的前提是树莓派的**上级路由能获取公网ip**。

## 其他的一些入坑tips
插上2.5寸移动硬盘 sudo fdisk -l 看不到硬盘目录的，需要在`/boot/config.txt`里面添加一行`max_usb_current=1` 增加USB输出电流，重启生效。

移动硬盘最好ext4格式的，配合aria2文件预分配方式falloc。

常用网站：

> [https://lug.ustc.edu.cn/wiki/mirrors/help/raspbian](https://lug.ustc.edu.cn/wiki/mirrors/help/raspbian) 中科大镜像站

> [https://lug.ustc.edu.cn/wiki/mirrors/help/archive.raspberrypi.org](https://lug.ustc.edu.cn/wiki/mirrors/help/archive.raspberrypi.org) 中科大镜像站

> [https://github.com/wwj718/awesome-raspberry-pi-zh](https://github.com/wwj718/awesome-raspberry-pi-zh) 树莓派的awesome系列 资源大全中文版

> [http://dietpi.com/phpbb/viewtopic.php?f=8&t=5#p5](http://dietpi.com/phpbb/viewtopic.php?f=8&t=5#p5) DietPi的软件列表

> [https://www.v2ex.com/go/pi](https://www.v2ex.com/go/pi) V2EX上关于树莓派的讨论
