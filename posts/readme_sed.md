---
title: sed
date: '2014-06-15 13:14:48'
permalink: 
description: 
categories: 
- 技术相关

tags: 
- tool
---

### 说明
摘录sed工具的使用

---

### url:
<pre>
http://blog.csdn.net/a81895898/article/details/8482387
http://coolshell.cn/articles/9104.html
http://tsnc.zhongaokao.com/tsnc_wgrj/doc/sed.htm
http://kodango.com/sed1line-notes
</pre>
	
### sed调试工具
	sedsed
	
### 正则：
> #### 规则
<pre>
^		表示一行的开头
$		表示一行的结尾
\<		表示词首。如\<abc表示以abc为首的词
\>		表示词尾。如\>abc表示以abc为尾的词
.		表示任意单个字符
*		表示某个字符出现了0次或多次
+		表示某个字符出现了1次或多次
[abc]	表示a或b或c字符
[^c]	表示非c的字符
&		表示被匹配的变量
\num	被匹配的第num个分组（\(...\)括起来的部分成为分组）
</pre>

> #### e.g.
<pre>
/[012]\{3\}/                    			###< 三个数字 [012]{3}
sed 's/xxx/[&]/g' my.txt					###< 匹配项加[]
s/(ACTIVE_CONSOLES=/dev/tty\[1-)6]/\12]/	###< ACTIVE_CONSOLES=/dev/tty[1-6]
											###< -> ACTIVE_CONSOLES=/dev/tty[1-2]
</pre>

											
### 工作形式
<pre>
sed [option] script file
script是由一条或多条指令(instruction)构成
指令由正则表达式(pattern)和编辑命令(action)构成
</pre>


### 工作过程
<pre>
以行为单位处理
每一行都是被读入到一块缓存空间，该空间名为模式空间(pattern space)
</pre>


### sed选项
> #### -e：选项可选，在命令行中同时指定多个操作指令时用到
<pre>
sed -e 's/x/y/' -e 's/y/x/' file
sed 's/x/y/;s/y/x/' file
</pre>

> #### -n：抑制默认输出（sed默认会把处理过的内容全输出）
<pre>
sed 's/x/y/p' file			默认输出全部内容，等于把匹配的行输出两遍
sed -n 's/x/y/p' file		只打印匹配的行
</pre>

> #### -i：对文件进行修改
<pre>
默认不修改文件，输出到stdout
作用类似 sed "s/my/jphome's/g" pets.txt > pets.txt
sed "s/my/jphome's/g" pets.txt			###< 输出文件到stdout，没有对文件进行修改
</pre>


### sed打印命令
<pre>
# p：打印匹配行 （类似于grep）
sed '/fish/p' my.txt		/// 会输出所有
sed -n 'fish/p' my.txt		/// 只打印匹配行

# =：打印行号
sed '/fish/=' my.txt

# l：打印行，打印在模式空间中的行，同时显示控制字符
</pre>


### sed转换命令
<pre>
# [address]y/SET1/SET2/
echo "hello world" | sed 'y/abcdefghijklmnopqrstuvwxyz/ABCDEFGHIJKLMNOPQRSTUVWXYZ/'		###< -> HELLO WORLD
sed 'y/abcdefghijklmnopqrstuvwxyz/ABCDEFGHIJKLMNOPQRSTUVWXYZ/' file
</pre>

### sed编辑命令
<pre>
# i：前面插入
# a：后面添加
# c：替换行
sed '1 i Insert' my.txt				###< 在第一行前插入一行xxx
sed '$ a Append' my.txt				###< 在最后一行后添加一行xxx
sed '/fish/a Append fish' my.txt 	###< 在匹配到fish的那行后面插入一行xxx
sed '2 c This is monkey'			###< 替换第二行为xxx
sed "/fish/c this is monkey"    	###< 替换匹配到fish的行
/fish/c\                        	###< 支持多行
xxx\                            	###< sed -i -f xxx.sed xxx.txt
xxx

# d：删除行
sed '/fish/d' my.txt		/// 删除匹配到fish的一行
sed '2d' my.txt			/// 删除第二行
sed '2,$d' my.txt		/// 删除第二到最后一行

# s：匹配替换(s/xx/xx/)
单引号内无法使用转义符/
# 只替换每一行的第一个xx
sed 's/xx/xx/1'
# 只替换每一行的第二个xx
sed 's/xx/xx/2
# 只替换每一行的第三个以后的xx
sed 's/xx/xx/3g'
# 替换每一行的所有匹配
sed 's/xx/XX/g'

# r: 将文件内容读入匹配行后面
# sed会把r后面的任何字符判断为文件名,直到回车或单引号，所以不能用;来连接其他语句
sed '/pattern/r file1' file						###< 将file1文件的内容添加到匹配行后面
sed '/pattern/{r file1' -e other cmd} file
sed '/pattern/{r file1
other cmd
}' file

# w: 将匹配到的内容写到文件
sed '/pattern/w file1' file
</pre>


### 保持空间
<pre>
# 小写覆盖、大写追加（追加内容与原内容以\n分隔）
h/H		将模式空间的内容复制/者追加到保持空间
g/G		将保持空间的内容复制/者追加到模式空间
x		交换模式空间和保持空间的内容
</pre>


### 其他
> #### !
<pre>
表示后面的命令对所有没有被选定的行发生作用
/pattern/!{}
</pre>

> #### 把12，34，56...行当成同一行来匹配
<pre>
sed 'N;s/\n//' my.txt		###< 就是把12，34，56...合并成一行
</pre>

> #### 修改指定行
<pre>
sed '3s/xx/xx/g' pets.txt	            ###< 第3行
sed '3,6s/xx/xx/g' pets.txt	            ###< 第3～6行
sed '1,${/this/d;s/^ *//g}' pets.txt
</pre>

> #### 在sed语句内使用变量
<pre>
sed -i 's/^xxx/xx=`$DIR`/' xxx.file
</pre>

> #### 条件正则
<pre>
sed '/^#define/{s/ //g}'
</pre>

> #### n: 读取下一个输入行,用下一个命令处理新的行
<pre>
/.H1/{n;/^$/d}		###< 删除.H1之后的第一个空行
</pre>

### 例子
<pre>
# 去除html文件中的tags
sed 's/<[^>]*>//g' index.html > index.html
# 行首加#
sed 's/^/#/g' pets.txt
# 行尾加 ---
sed 's/$/ ---/g' pets.txt
# 使用逗号,拼接行
sed 'H;$!d;${x;s/^\n//;s/\n/,/g}' file

# 命令打包
sed '4,6 {/This/{/fish/d}}' pets.txt
</pre>


