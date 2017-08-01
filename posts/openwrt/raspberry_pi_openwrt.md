---
title: Raspberry Pi烧写Openwrt固件
date: '2014-08-01 21:55:26'
permalink: 
description: 
categories: 
- 技术相关

tags: 
- Raspberry Pi
- OpenWrt
---


### 说明
给树莓派做上Openwrt固件学习luci编程

<hr/>

### 1.下载Openwrt固件、启动补丁文件

    固件：openwrt-brcm2708-sdcard-vfat-ext4_224.img
    http://downloads.openwrt.org/attitude_adjustment/12.09/brcm2708/generic/
    启动补丁：使用OPENWRT的正确姿势.zip
    http://pan.baidu.com/share/link?shareid=496011089&uk=1328231555

### 2.烧写固件
插上SD卡

    sudo umount /dev/sdbn           ###< 卸载/dev/sdb的所有分区
    sudo fdisk /dev/sdb             ###< 删除/dev/sdb的所有分区
    sudo dd bs=1M if=openwrt-brcm2708-sdcard-vfat-ext4_224.img of=/dev/sdb
完成后使用补丁文件bootcode.bin、start.elf替换SD卡FAT分区的启动文件

<hr/>

### Refs
* [http://show.smzdm.com/detail/41689](http://show.smzdm.com/detail/41689)

