---
title: git
date: '2013-10-16 21:37:03'
permalink: 
description: 
categories: 
- 技术相关

tags:
- tool
---


### 1. 安装
<pre>
sudo apt-get install git git-core
</pre>


### 2. 配置 设置你的名字和email(提交commit时的签名)
<pre>
git config --global user.name "jphome"
git config --global user.email "jphome98@gmail.com"
</pre>


#### 执行了上面的命令后,会在你的主目录(home directory)建立一个叫 ~/.gitconfig 的文件
##### 内容一般像下面这样:
<pre>
[user]
    name = jphome
    email = jphome98@gmail.com
</pre>

注:这样的设置是全局设置,会影响此用户建立的每个项目.

### 3. git命令
<pre>
git clone https://github.com/jphome/hSmart.git
git add *
git commit
git remote add origin https://github.com/jphome/hSmart.git
git push -u origin master


本地编辑前
git pull
如果有冲突就会提示 处理冲突
然后 git ci -a
然后 git push 同步github上的数据


编辑完了后
git ci -a
git push


当本地与远程冲突时
git fetch origin            ###< 获取冲突
git merge origin            ###< 如果merge失败 手动编辑修改
git ci -a
git add *
git push


git init
git add file1 file2 file3   ###< 将更新的内容（修改 & 新增）添加到索引中

git commit -a               ###< 自动把所有内容被修改的文件(不包括新创建的文件)都添加到索引中,并且同时把它们提交
git diff --cached           ###< 查看哪些文件将被提交（commit）
git diff                    ###< 查看当前你所有已做的但没有加入到索引里的修改
git diff HEAD               ###< 显示你工作目录与上次提交时之间的所有差别
git commit                  ###< 提交到本地


git push                    ###< 推到网上仓库
git pull                    ###< 从远程仓库down更新 = git fetch + git merge
                                ###< 当本地与远程都编辑了 git pull就会失败


git branch experimental     ###< 新建分支
git branch                  ###< 查看所有分支（*为当前所处分支）
git checkout experimental   ###< 切换分支
git merge experimental      ###< 合并分支experimental到当前分支
git branch -d experimental  ###< 删除分支experimental
gitk                        ###< 图形界面显示项目历史
git reset --hard HEAD       ###< 回到合并之前的状态
git diff master..test       ###< 显示两个分支间的差异
git diff master...test      ###< 显示‘master’,‘test’的共有 父分支和'test'分支之间的差异
</pre>


### 4. 下载git仓库的文件/文件夹
<pre>
wget --no-check-certificate https://raw.githubusercontent.com/jphome/WRT_package/master/helloworld/src/helloworld.c
    https://github.com/qdk0901/openwrt-mt7620/tree/master/package/rt2860v2
svn checkout https://github.com/qdk0901/openwrt-mt7620/trunk/package/rt2860v2
</pre>

