---
title: ubuntu12.04安装PPStream
date: '2013-10-27 17:58:45'
permalink: 
description: 
categories: 
- 生活

tags: 
- tool
---

### 简介:
摘自about: PPS网络电视是一款基于P2P技术的流媒体直播软件,能够为宽带用户提供稳定和流畅的视频直播节目。PPS网络电视采用P2P-Streaming技术的流媒体直播

---

### 1.安装qt4依赖包
    sudo apt-get install libqt4-core libqt4-gui libqt4-webkit

### 2.[解决Ubuntu12.04下PPStream一直缓冲后跳到下一个问题](http://dawndiy.com/archives/52/)
    sudo apt-get install libjpeg62

### 3.下载安装包
    wget http://download.ppstream.com/linux/PPStream.deb

### 4.安装PPStream
    sudo dpkg -i PPStream.deb

