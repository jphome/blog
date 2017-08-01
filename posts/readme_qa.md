---
title: OpenWrt常见疑问
date: '2014-08-13 23:30:46'
permalink: 
description: 
categories: 
- 技术相关

tags: 
- OpenWrt
---

### 说明
在使用基于OpenWrt开发的时候会碰到很多问题，汇总方便查阅

<hr/>

#### openwrt的wlan0接入其他路由器上网
<pre>
1. 将openwrt的LAN口地址配成192.168.2.1
2. WWAN配成DHCP客户端
3. 在无线中搜索热点
</pre>

<hr/>

#### 通过有线能连openwrt的ssh,wifi不能
<pre>
1. openwrt通过Dropbear提供ssh&scp服务
2. 配置防火墙中WWAN口入路由器的权限
</pre>

<hr/>

#### 配置路由器启动默认开启wifi
<pre>
trunk/package/mac80211/files/lib/wifi/mac80211.sh
option disabled 1
</pre>

<hr/>

#### 单独编译内核模块安装包
<pre>
make package/kernel/xxx {compile, install} V=s
PS: OpenWrt的kernel modules配置文件都在这儿
</pre>

<hr/>

#### 添加U盘/移动硬盘支持
<pre>
添加USB挂载
Base system —> <*>block-mount   
添加硬盘格式支持（）
Kernel modules —> Filesystems —> <*> kmod-fs-ext4 (移动硬盘EXT4格式选择)
Kernel modules —> Filesystems —> <*> kmod-fs-vfat(FAT16 / FAT32 格式 选择)
Kernel modules —> Filesystems —> <*> kmod-fs-ntfs (NTFS 格式 选择)
添加UTF8编码,CP437编码，ISO8859-1编码
Kernel modules —> Native Language Support —> <*> kmod-nls-cp437
Kernel modules —> Native Language Support —> <*> kmod-nls-iso8859-1
Kernel modules —> Native Language Support —> <*> kmod-nls-utf8
添加SCSI支持
Kernel modules —> Block Devices —> <*>kmod-scsi-core
添加USB相关支持
Kernel modules —> USB Support —> <*> kmod-usb-core.
Kernel modules —> USB Support —> <*> kmod-usb-ohci.
Kernel modules —> USB Support —> <*> kmod-usb-storage.
Kernel modules —> USB Support —> <*> kmod-usb-storage-extras.
Kernel modules —> USB Support —> <*> kmod-usb2.
添加自动挂载工具
Utilities —> Filesystem —> <*> badblocks
</pre>

<hr/>

#### 取消strip
<pre>
make package/xxx {clean, compile} V=99 STRIP=/bin/true
也就是说如果默认使用strip破坏了你的程序、库，可以使用STRIP=/bin/true来取消strip操作，直接在Makefile中定义也是可以的
</pre>

<hr/>

#### U-boot移植编译
<pre>
http://www.right.com.cn/forum/thread-84684-1-1.html
</pre>

<hr/>

#### 4G LTE的移植
<pre>
大致步骤 移植4G网卡，改写网络配置文件，改写4G拨号脚本，配置WIFI和WIFI的DHCP。
涉及文件：
内核部分   driver/usb/serial/option.c   加PID,VID
文件系统部分： 
/etc/config/network   加入WAN接口配置并配置为4G模式；加入WIFI接口，并配置为静态地址模式以便能自动启。
/etc/config/wireless  将wifi-iface的network字段与 /etc/config/network中的WIFI接口匹配相同
/etc/config/firewall    修改防火墙规则，使各个接口都可以通信。
/etc/config/dhcp      添加WIFI接口的DHCP功能
/etc/chatscripts/3g.ch  拨号脚本
 以上所有文件内容在这个网页：
 http://blog.csdn.net/sydjm/article/details/8490357
A、4G网卡移植，参照以下，把网卡的PID VID写进内核中，否则可能不能转换MODEM模式。
 http://wiki.openwrt.org/zh-cn/doc/recipes/3gdongle
 内核可能需要添加USB存储支持和块设备的SCSI支持。
 http://www.hkepc.com/forum/viewthread.php?tid=1712898
 http://www.hkepc.com/forum/viewthread.php?tid=1712898&rpid=26287429&ordertype=0&page=1#pid26287429
 http://www.hkepc.com/forum/viewthread.php?tid=1661598
 移植4G的APN是 CMNET ，需要写进/etc/config/network,/etc/chatscripts/3g.chat
B、配置WIFI及其DHCP
http://wiki.openwrt.org/doc/recipes/routedap
</pre>

<hr/>

#### SVN版本控制
<pre>
svn 回滚 或 svn 指定版本  
需要的版本是 28007
操作：
1. SVN 最新的代码 ： svn co svn://svn.openwrt.org/openwrt/trunk -r 28007
2. 回滚或到指定版本：svn up -r 28007
指定LUCI版本
cd trunk/feeds/luci 
svn up -r r7612
</pre>

<hr/>

#### 添加复位键支持
<pre>
Utilities  ---> <*> restorefactory
添加一键开关无线
Utilities  ---> <*> wifitoggle
</pre>

<hr/>

### Refs
* http://blog.csdn.net/sydjm/article/details/8232963

