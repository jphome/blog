---
title: readme_python
date: '2013-10-16 23:24:31'
permalink: 
description: 
categories: 
- 技术相关

tags:
- language
---


### python模块Package Index:
https://pypi.python.org/pypi


### 从源码安装python模块
    python setup.py build
    sudo python setup.py install


### python第三方模块管理工具:pip
#### 安装:
##### 首先安装setuptools
    wget http://pypi.python.org/packages/source/s/setuptools/setuptools-0.6c11.tar.gz
##### 安装pip
    wget https://pypi.python.org/packages/source/p/pip/pip-1.4.1.tar.gz

### 使用pip:
#### 安装第三方模块
    pip install xxx
#### 升级模块
    pip install --upgrade xxx
#### 移除模块
    pip uninstall xxx


### [性能评估](http://www.lupaworld.com/article-229520-1.html)

#### 粗粒度的计算时间
    time python xxx.py
        real:       实际花费的时间
        user:       cpu花费在内核外的时间
        sys:        cpu花费在内核内的时间
        user+sys: cpu在程序上花费的时间
    如果user+sys<<real,则程序的大部分性能瓶颈在IO等待上


### 模块
PYTHONPATH包含模块所在路径


### package
包就是模块所在的路径<br \>
#### 比如有两个模块 color.py shape.py
    PYTHONPATH=.:$(PYTHONPATH)
    ./draw/__init__.py
    ./draw/color.py
    ./draw/shape.py

    import draw;            ###< __init__.py生效,coloe/shape不可用
    import draw.color;      ###< color可用,但得用全名:draw.color

