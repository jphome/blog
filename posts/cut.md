---
title: cut工具的使用方法
date: '2013-10-23 22:12:30'
permalink: 
description: 
categories: 
- 技术相关

tags:
- tool
---


## cut作用:用来裁剪数据
#### 参数
    -b      字节(bytes)
    -c      字符(characters)
    -f      域(fields)



## 1. 按字节(bytes)
#### 一个汉字算3个字节
    date | cut -b 1-4           ###< 取前4B
    date | cut -b 1-4,10        ###< 取前4B和第10个B
    date | cut -b -4            ###< 取1-4B
    date | cut -b 4-            ###< 取第4个B到行尾

## 2. 按字符(characters)
#### 中文字符和空格都算一个字符
    date | cut -c 1-5


## 3. 按域(fields)
#### -d 指定域分隔符(默认为Tab)
#### -f 指定要剪出哪几个域[1, 2, 3, 4...]
    head -n5 /etc/passwd | cut -d: -f1,3-5      ###< 对passwd文件的前5行进行裁剪
    cut -d":" -f2                               ###< 空格表示

