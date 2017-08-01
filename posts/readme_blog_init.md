---
title: gor搭建个人博客(github静态博客)
date: '2013-10-13 22:20:45'
permalink: 
description: 
categories: 
- 技术相关

tags:
- blog
---

### 简介:
以前习惯把积累的东西用readme文件的形式存在本地,方便查阅
现在打算整理成静态blog的形式,简单方便,走到哪儿都可以查阅

---

静态博客 [[gor](https://github.com/wendal/gor) -- Golang编写]


### 安装gor
    go get -u github.com/wendal/gor
    go install github.com/wendal/gor/gor


### 新建站点 生成jphome98.com文件夹
    gor new jphome98.com


### 新建单篇博客
    cd jphome98.com
    gor post "readme"           ###< 生成博客页 posts/readme.md
                                ###< 编辑 readme.md
                                ###< 当然也可以编辑好xxx.md直接放到 posts下

### 编译
    gor compile                 ###< 生成的资源放在compiled文件夹


### 本地预览
    gor http
    浏览器输入localhost:8080


### 部署到github上   || e.g: [hugozhu](https://github.com/hugozhu/blog)
    在github上新建repo "jphome.github.com"
    将compiled提交到此repo
    即可通过http://jphome.github.com访问


### 基本配置
打开站点根目录下的site.yml文件

    1. 填入title, 作者等信息
    2. 填入邮箱等信息

打开站点根目录下的config.yml文件

    1. 设置production_url为你的网站地址, 例如 http://wendal.net 最后面不需要加入/ 生成rss.xml等文件时会用到
    2. summary_lines 首页的文章摘要的长度,按你喜欢的呗
    3. latest 首页显示多少文章
    4. imgs 部分为自动插入 <img> 相关的配置
        imgtag：要插入的  标签的基本格式，%s 部分会被自动替换为 urlperfix/post_name/img_file_name 的格式
        urlperfix： 图片地址前缀
        localdir：图片文件在博客内的本地存放目录

打开widgets目录, 可以看到基本的挂件,里面有config.yml配置文件

    1. analytics 暂时只支持google analytics, 填入tracking_id
    2. comments 暂时只支持disqus, 请填入short_name
    3. google_prettify 代码高亮,一般不修改

