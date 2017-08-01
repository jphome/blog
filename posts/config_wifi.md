---
title: 树莓派USB无线网卡配置
date: '2013-10-15 21:03:24'
permalink: 
description: 
categories: 
- 技术相关

tags: 
- Raspberry Pi
---


### 说明
配置树莓派wifi，开机连上路由器

参考: [http://www.2fz1.com/?p=353](http://www.2fz1.com/?p=353)
<hr/>

购置无线网卡
手头本来有一个RT3070,可是怎么都搞不定
后淘了个[EDUP的EP-N8508GS](http://detail.tmall.com/item.htm?id=14795585956)

### 1.查看插入的无线网卡驱动有没有加载
    lsusb
#### 我的显示如下无线网卡信息
    Bus 001 Device 004: ID 0bda:8176 Realtek Semiconductor Corp. RTL8188CUS 802.11n WLAN Adapter

### 2.配置网卡配置文件
    cd /etc/network
    cp interfaces interfaces.bak
    sudo vi interfaces 
#### 内容如下
    auto lo
    iface lo inet loopback
    iface eth0 inet dhcp

    auto wlan0
    allow-hotplug wlan0
    iface wlan0 inet dhcp
    wpa-ssid "SSID"
    wpa-psk "PASSWORD"

### 3.配置路由器为树莓派静态分配ip

<br \>

##### 存在的问题: 网卡插入后设备自动重启,略讨厌,不过总归有网上了

