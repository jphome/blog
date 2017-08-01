---
title: vim的相关使用
date: '2013-11-06 22:58:35'
permalink: 
description: 
categories: 
- 技术相关

tags:
- tool
---

### 说明
摘录vim编辑器的使用

---

### vim模式
* nromal
* visual（选择）
* insert
* cmd-line/Ex
    * ':'进入cmd-line模式
    * 'Q'进入Ex模式（多cmd-line模式）


### 常规插件安装方法
<pre>
下载插件包xxx.zip
解压zip： xxx.txt -> ~/.vim/doc
xxx.vim -> ~/.vim/plugin
进~/.vim/doc
:helptags .                 ###< 把新的xxx.txt加入helptags
</pre>


### map命令相关：
> #### 在设置map时按键名
<pre>
<C-a>       Ctrl+a
<A-a>       Alt+a
<C-A-a>     Ctrl+Alt+a
</pre>

> #### map命令前缀
<pre>
默认的map命令影响到normal和visual模式
nore        表示非递归
n           表示在普通模式下生效
v           表示在可视模式下生效
i           表示在插入模式下生效
c           表示在命令行模式下生效
</pre>

> #### 非递归说明
<pre>
:map a b
:map c a
对于c等效于
:map c b
:noremap c a                   ###< 这种就指定了非递归map
</pre>

> #### 清除map
<pre>
:unmap c                       ###< 删除映射
:mapclear                      ###< 直接清除相关模式下的所有映射
</pre>

### taglist.vim中使用的tags生成
<pre>
ctags -R ./src              ###< 生成tags
ctags -R *
</pre>

### 保存当前的编辑状态
###### 要恢复上次的编辑环境，需要保存session或viminfo
> #### 使用Session
<pre>
:mksession [file.vim]       ###< 保存session,默认session文件名为Session.vim
:mksession!                 ###< 已存在Session.vim的情况下
:source Session.vim         ###< 恢复vim状态
vim -S Session.vim          ###< 启动vim时回复session
</pre>

> #### 使用viminfo
<pre>
:wviminfo hsmart.viminfo    ###< 保存viminfo
:rviminfo hsmart.viminfo    ###< 读入保存的viminfo文件
</pre>

### 设置编码格式
<pre>
encoding:       vim内部使用的字符编码方式，包括viｍ的buffer、菜单文本、消息文本等
fileencoding:   vim中当前编辑的文件的字符编码方式，vim保存文件时也会将文件保存为这种编码方式
fileencodings:  vim启动时会按照它所列出的字符编码方式逐一探测即将打开的文件的字符编码方式，并且将fileencoding设置为最终探测到的字符编码方式。因此最好将unicode编码方式放到这个列表的最前面，将拉丁语系编码方式latin1放到最后面
</pre>


### 函数
###### 内部调用还是得声明在先
<pre>
:call test()

func! test()
endfunc

func test()
endfunc

function! Test()
  if
  else
  endif
endfunction
</pre>


### 插件管理vundle
<pre>
git clone https://github.com/gmarik/vundle.git ~/.vim/bundle/vundle
:BundleInstall
</pre>

### 杂项
<pre>
Ctrl+z/:suspend                ###< 在shell下是挂起vim; gui下是最小化窗口
:!{command}                    ###< 执行shell命令
:shell                         ###< 开一个新的shell
:!time {command}               ###< 跑完显示运行时间
:r !date                       ###< 将shell执行结果读到正在编辑的文件
:1,4!sort                      ###< 将几行排序
:set fileencoding              ###< 查看变量
:verbose map <C-A>             ###< 获取C-A快捷键的绑定内容
</pre>

