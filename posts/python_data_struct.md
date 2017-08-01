---
title: python数据结构
date: '2013-11-07 22:44:06'
permalink: 
description: 
categories: 
- 技术相关

tags:
- language
---

```
#!/usr/bin/env python
# -*- coding=utf-8 -*-
#########################################################################
# File Name: data_struct.py
# Author: jphome
# mail: jphome98@gmail.com
# Created Time: Fri 06 Sep 2013 10:16:51 PM CST
#########################################################################
```

### 字符串
* 单引号'
* 双引号"
* 三引号'''或"""      ###< 可以多行，中间可以使用'和"
###### 转义 'What\'s ' 'your name?'     ==>     'What\'s your name?'

```
name = 'Swaroop';
if name.startswith('Swa'):
    print 'xxx';
if 'a' in name:
    print 'xxx';
if name.find('war') != -1:
    print 'xxx';
delimiter = '_*_';
mylist = ['B', 'R', 'I', 'C'];
print delimiter.join(mylist);	###< B_*_R_*_I_*_C
```


### 列表list
###### 列表可修改
```
list = ['apple', 'mango'];
print list;                 ###< 打印 ['apple', 'mango']
len(list);                  ###< item个数
for item in list:           ###< 遍历list
    print item;
list.append('rice');        ###< 添加item
list.sort();                ###< 排序list
olditem = list[0];          ###< 提取item
del list[0];                ###< 删除item

list.extend(L);				###< 用链表来扩充链表
							###< 相当于list[len(list):] = L
list.remove(x);				###< 移除值为x的元素
list.pop(3);				###< 
list.pop();					###< 弹出最后一个元素
list.index(x);				###< 返回列表中第一个值为x的索引
list.count(x);				###< 返回x在列表中出现的次数
list.reverse();				###< 列表逆序
```

> #### 堆栈stack
```
stack = [];
stack.append(3);
stack.pop();
```

> #### 队列queue
```
queue = [];
queue.append(4);
queue.pop(0);
```

> #### 列表综合
```
listone = [2, 3, 4];
listtwo = [2*i for i in listone if i>2];    ###< [6, 8]
```

> #### 列表切片
```
a = [0,1,2,3,4,5,6,7,8,9];
a[2:4];                         ###< a[2]-a[3]
a[:3];                          ###< a[0]-a[2]
a[1:6:2];                       ###< a[1] a[3] a[5] 步进为2
a[i:j:s];                       ###< i默认为-1,j默认为-len(a)-1
a[::-1];                        ###< a[-1:-len(a)-1:-1] 其实就是列表逆转
```


### 元组
###### 元组不可变,无法修改
```
empty = ();                     ###< 空元组
singleton = (2, );              ###< 单元素的元组,必须加,

zoo = ('wolf', 'elephant');
print zoo;                      ###< 打印 ('wolf', 'elephant')
len(zoo);                       ###< item个数
new_zoo = (zoo, 'monkey');      ###< 转移
print new_zoo;                  ###< (('wolf', 'elephant'), 'monkey')
print new_zoo[0][0];            ###< wolf
```
```
# print格式化输出
age = 22;
name = 'jph';
print '%s is %d years old' % (name, age);           ###< 使用元组定制字符串
print '%s is playing python' % name;                ###< 只需要一个参数时可以不加()
```


### 字典dict
###### 键值对,键必须唯一
######  只能使用不可变的对象(比如字符串)作为键
######  形式: d = {key1:val1, key2:val2};
```
dict = {
    'j' : "jj",
    'p' : 'pp',
    'h' : 'hh'
};
dict['j'] = 'jb';                           ###< 修改item
del dict['p'];                              ###< 删除item
len(dict);                                  ###< item个数
keys = dict.keys();
vals = dict.values();
xxx = dict.get('a', "a_default_val");       ###< 查找'a'对应的val,没有则返回"a_default_val"
for key, val in dict.items():               ###< 遍历dict
    print 'Contact %s => %s' % (key, val);
if 'j' in dict:                             ###< 判断是否存在key
    print "j's val is %s" % dict['j'];
if dict.has_key('j'):                       ###< 判断是否存在key
    print "j's val is %s" % dict['j'];
```

### 序列
###### 列表,元组,字符串都是序列
###### 序列的 索引操作符 & 切片操作符 (index & slicing)
```
list = ['apple', 'mango', 'banana', 'pan'];
list[0];                    ###< 访问第一个item
list[-1];                   ###< 访问最后一个item
list[-2];                   ###< 访问倒数第二个item
list[1:2];                  ###< 切片 [1]
list[:3];                   ###< 从头到[2]
list[2:];                   ###< [2]到尾
```


### 参考
###### 即变量名,指向对象存储的内存
```
list = ['a', 'b', 'c'];
mylist1 = list;                 ###< 别名
mylist2 = list[:];              ###< 切片拷贝
del mylist1[0];                 ###< 会影响到list
del mylist2[0];                 ###< 不会影响到list
```


### 集合set
```
s = set('ab');                  ###< set(['a', 'b'])
len(s);
if 'a' in s:
if 'c' not in s:
if s.issubset(ss):              ###< 判断是否s为ss的子集
if s <= ss:                     ###< 判断是否s为ss的子集
if ss.issuperset(s):            ###< 判断是否ss为ss的超集
if ss >= s:                     ###< 判断是否ss为ss的超集
u = s.union(t);                 ###< 返回并集
u = s | t;                      ###< 返回并集
a = s.intersection(t);          ###< 返回交集
a = s & t;                      ###< 返回交集
s.difference(t);                ###< 返回s 中有但是 t 中没有的
s.symmetric_difference(t)       ###< 返回 s 和 t 中不重复的元素
s ^ t                           ###< 返回 s 和 t 中不重复的元素
```

