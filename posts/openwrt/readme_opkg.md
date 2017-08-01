---
title: 用u盘扩展openwrt路由器存储
date: '2014-03-29 21:36:20'
permalink: 
description: 
categories: 
- 技术相关

tags: 
- OpenWrt
---


### 说明
由于flash太小,可以用<code>读卡器+sd卡/U盘/移动硬盘</code>来扩展路由器的存储
<hr/>
#### 1. 将usb存储格式化成ext3文件系统

    mkfs.ext3 /dev/sdb

#### 2. 挂载sd卡

    mkdir /mnt/sda1
    mount -t ext3 /dev/sda1 /mnt/sda1
    mkdir /mnt/sda1/packages

#### 3. 修改opkg配置文件

    echo dest usb /mnt/sda1/packages >> /etc/opkg.conf

#### 4. opkg更新

    opkg update

#### 5. 修改PATH环境变量
vim /etc/profile

    export PATH=$PATH:/mnt/sda1/packages/usr/bin:/mnt/sda1/packages/usr/sbin

#### 6. 安装软件

    opkg --dest usb install python
    opkg --dest usb install helloworld.ipk

