---
title: OpenWrt版本&代码路径
date: '2014-08-18 23:12:02'
permalink: 
description: 
categories: 
- 技术相关

tags: 
- OpenWrt
---

### 说明
OpenWrt版本分开发主干 + 稳定版

<hr/>

### OpenWrt版本路线图
<br/>
![OpenWrt版本路线图](/assets/media/img/openwrt_version.png "OpenWrt版本路线图")

### 各大版本源码
> #### 主干开发分支trunk
##### Barrier Breaker
    git clone git://git.openwrt.org/openwrt.git
    svn co svn://svn.openwrt.org/openwrt/trunk/

> #### 稳定分支branches
>> ##### Kamikaze 7.09
    svn co svn://svn.openwrt.org/openwrt/tags/kamikaze_7.09
>> ##### Kamikaze 8.09
    svn co svn://svn.openwrt.org/openwrt/branches/8.09
>> ##### Backfire 10.03
    svn co svn://svn.openwrt.org/openwrt/branches/Backfire
>> ##### Attitude Adjustment 12.09
    svn co svn://svn.openwrt.org/openwrt/branches/attitude_adjustment


<hr/>
### Refs
* http://www.cnblogs.com/double-win/p/3838112.html
* http://wiki.openwrt.org/about/history
* https://dev.openwrt.org/wiki/GetSource
