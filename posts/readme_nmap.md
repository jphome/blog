---
title:  Linux 下查看局域网内所有主机IP和MAC
date: '2014-03-29 23:03:54'
permalink: 
description: 
categories: 
- 技术相关

tags: 
- tool
---

### 说明
用nmap对局域网扫描一遍，然后查看arp缓存表就可以知道局域内ip对应的mac了。nmap比较强大也可以直接扫描mac地址和端口。执行扫描之后就可以 cat /proc/net/arp查看arp缓存表了。
<br/>
<br/>
转自[残剑](http://blog.chinaunix.net/uid-25885064-id-3483813.html)

<hr/>

#### 进行ping扫描，打印出对扫描做出响应的主机：
<pre>
$ nmap -sP 192.168.1.0/24
</pre>

#### 仅列出指定网络上的每台主机，不发送任何报文到目标主机：　
<pre>
$ nmap -sL 192.168.1.0/24
</pre>

#### 探测目标主机开放的端口，可以指定一个以逗号分隔的端口列表(如-PS 22，23，25，80)：
<pre>
$ nmap -PS 192.168.1.234
</pre>

#### 使用UDP ping探测主机：
<pre>
$ nmap -PU 192.168.1.0/24
</pre>

#### 使用频率最高的扫描选项（SYN扫描,又称为半开放扫描），它不打开一个完全的TCP连接，执行得很快：
<pre>
$ nmap -sS 192.168.1.0/24
</pre>

