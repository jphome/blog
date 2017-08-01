---
title: 安装使用slickedit
date: '2014-04-13 10:06:37'
permalink: 
description: 
categories: 
- 技术相关

tags: 
- tool
---

### 类似<code>SourceInsight</code>编辑器,尝个鲜
<hr/>

#### 官网下载v18.0.1版本

    wget http://www.slickedit.com/assets/trial/se_18000102_linux32.tar.gz

#### 安装
<pre>
    tar xvf se_18000102_linux32.tar.gz
    cd se_18000102_linux32 
    sudo ./vsinst
    安装到/opt/slickedit路径下
</pre>

#### 破解 [REF](http://blog.book41.net/?p=751)
<pre>
    cd /opt/slickedit/bin
    sudo cp vs_exe vs_exe.bak       ###< 备份
    sudo chmod 775 vs_exe           ###< 加w权限
    sudo vim vs_exe -b              ###< 编辑二进制,不加换行符
    :%!xxd                          ###< 进入十六进制编辑模式
    /009D370                        ###< 搜索地址
    修改5个字节                     ###< 具体见下图
    :%!xxd -r                       ###< 转回二进制
    :wq                             ###< 保存退出
</pre>

![from](/assets/media/img/img_readme_slickedit_0.png)
<br/>
<br/>
![to](/assets/media/img/img_readme_slickedit_1.png)

