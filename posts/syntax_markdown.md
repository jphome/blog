---
title: 学习markdown语言语法
date: '2013-10-13 22:45:44'
permalink:
description: 
categories: 
- 技术相关

tags:
- language
---


From: [basic](http://wowubuntu.com/markdown/basic.html)

### 回车
    <br />

### 标题(两种语法)
* _Setext_: 底线形式 === 或 ---
* _atx_: 在行首插入 1 到 6 个 #

### 修辞和强调
    *emphasized*        ###< 斜体
    _emphasized_        ###< 斜体
    **strong**          ###< strong
    __strong__          ###< strong

### 行内代码
    使用反引号'`'包裹起来

### 列表(* + - num)
    * 1                 ###< 无序列表
    * 2
    * 3

    + 1                 ###< 无序列表
    + 2
    + 3

    - 1                 ###< 无序列表
    - 2
    - 3

    1. a                ###< 有序列表
    2. a
    3. a

### 文字链接
    [xxx's blog](http://xxx.com "title")    ###< 行内形式

    [xxx's blog][id]                        ###< 参考形式
    [id]: http://url_1.com

### 图片链接
#### (打算在blog中使用yunio网盘的图片外链,尚未成功 - - 貌似不被支持了)
    ![alt text](/path/to/img.jpg "title")   ###< 行内形式

    ![alt text][id]                         ###< 参考形式
    [id]: /path/to/img.jpg "title"
<br />
<br />

### 测试几张外链的图片
![yunio](http://pic.baike.soso.com/p/20130304/bki-20130304161213-1881616484.jpg "yunio") Yunio网盘,竟然升级到1T了


![hehe][monkey_1]
![hehe][monkey_2]
[monkey_1]: http://forum.csdn.net/PointForum/ui/scripts/csdn/Plugin/003/monkey/1.gif
[monkey_2]: http://forum.csdn.net/PointForum/ui/scripts/csdn/Plugin/003/monkey/2.gif

