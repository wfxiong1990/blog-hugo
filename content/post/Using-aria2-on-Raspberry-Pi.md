---
title: 在树莓派上使用aria2
date: 2017-02-04 13:50:49
tags: [raspberry pi,aria2]
---

## 认识aria2

国内最流行的网盘是什么？百度盘，各种资源分享，当年申请就有2T容量，不敢用作资料备份，但偶尔下下电影蛮好的。后来限制了下载速度，后来强制要百度盘的客户端才能下载，于是就了解了chrome插件`网盘助手`,可以方便的把网盘的下载地址导出到aria2/aria2-rpc，支持YAAW
<https://github.com/acgotaku/BaiduExporter>。所以就开始了解aria2。

<!-- more -->

## 强大的aria2

官网 <http://aria2.github.io/>

> aria2 is a lightweight multi-protocol & multi-source command-line download utility. It supports HTTP/HTTPS, FTP, SFTP, BitTorrent and Metalink. aria2 can be manipulated via built-in JSON-RPC and XML-RPC interfaces.

看介绍就眼花缭乱了，轻量级，支持多种协议，多源头，可以下载http，bt，磁力链。总结就两个字，牛掰。

对linux比较熟悉的可以从<https://github.com/aria2/aria2/> 下面git clone 源码进行编译，看这步骤我也是没有信心，还好有人编译好了放在网上<https://github.com/q3aql/aria2-static-builds>。安装只需要三步，解压，进目录，make install。。。

当然也可以`sudo apt-get install aria2 –y`只是从中科大的镜像站安装的版本是1.18，最新版到了1.31了，略不爽。

其他的配置我是按照<http://mkitby.com/2016/01/15/raspberry-pi-nas-remote-download-aria2/>来的。

aria2需要web前端管理，我走了`aria2c.com`，把JSON-RPC Path加进去就好，为什么不在树莓派上搭对应的web server，懒ORZ。。。

关于文件预分配，我把移动硬盘格式化成了ext4格式，然后对应的`file-allocation=falloc`

对应的说明：<http://aria2.github.io/manual/en/html/aria2c.html>

>  If you are using newer file systems such as ext4 (with extents support), btrfs, xfs or NTFS(MinGW build only), **falloc** is your best choice. It allocates large(few GiB) files almost instantly. 

下载磁力链需要设置dht.dt的位置
`dht-file-path=/home/pi/.aria2/dht.dat`

bt下载慢，设置bt客户端伪装，伪装成uTorrent，
```
user-agent=uTorrent/2210(25130)
peer-id-prefix=-UT2210-
```
然后添加bt-tracker，推荐如下：

> <https://newtrackon.com/list>

> <https://github.com/ngosang/trackerslist>

写个小程序，把非空行用英文逗号隔开，放在aria2.conf里面的bt-tracker= 的后面，`sudo service aria2c restart`即可。

为什么不用transmission？ 相比aria2，transmission比较耗资源，连接数大后容易让树莓派死机，还是在x86平台上用transmission比较合适。

哦，对了。说到百度盘，推荐一个看上去干净网盘搜索<http://www.fastsoso.cn/>，一般想看优酷付费电视剧，在上面搜搜就好。如果链接被百度ban了，看这里<http://jingyan.baidu.com/article/359911f50ab22157fe0306c8.html>

*本文完*。
