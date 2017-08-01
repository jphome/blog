---
title: LuCI编程
date: '2014-08-16 09:25:12'
date: '2014-08-17 19:33:09'
permalink: 
description: 
categories: 
- 技术相关

tags:
- OpenWrt
- LuCI
---

### 说明
LuCI API
<br/>
LuCI template
<br/>
uci usage

<hr/>

### LuCI template
#### 内部常量
<pre>
REQUEST_URL         当前URL_
controller          主分发路径
resource            资源目录路径
media               主题目录路径
</pre>

<pre>
<%+template%> or <%include(template)%>              ###< 引用外部模板
    <%+header%>                                     ###< /usr/lib/lua/luci/view/header.htm
<%=value%> or <%write(value)%>                      ###< 插入变量值
<%:xxx Page%> or <%=translate("xxx Page")%>         ###< 翻译

<%code%>                                            ###< 插入lua代码
<%- xxx -%>                                         ###< 分行代码

<%#comment%>                                        ###< 注释
<%# xxx -%>                                         ###< 分行注释
</pre>

<hr/>

### LuCI API
http://luci.subsignal.org/api/luci/index.html

<hr/>

### uci usage
#### uci在读取的时候优先显示内存中的缓存，其次显示文件中的，其操作方式类似sqlite
<pre>
### uci命令
config xx1 xx2
    option xx3 xx4

xx2    section
xx1    section type
xx3    option名
xx4    option值

$ touch demo
$ uci add demo stype            ###< 添加一个匿名的section
=> config stype
$ uci set demo.sec=stype        ###< 添加一个stype类型的section
=> config stype 'sec'
$ uci set demo.sec.opt=val      ###< 添加一个option
=> config stype 'sec'
        option opt 'val'
$ uci add_list demo.sec.l=11    ###< 添加一个list
=> config stype 'sec'
        list l '11'
$ uci delete demo.sec.l         ###< 删除一个list
$ uci delete demo.sec.opt       ###< 删除一个option
$ uci delete demo.sec           ###< 删除一个section
$ uci delete demo.@stype[0]     ###< 删除section type为stype的地一个匿名section

### 导出配置
uci export > config_total                   ###< 导出所有配置
uci export netowrk > config_network         ###< 只导出/etc/config/network

### 设置lan ip
uci set network.lan.ipaddr=192.168.2.1

### 设置wan口pppoe拨号
uci set network.wan.proto=pppoe
uci set network.wan.username=xxx
uci set network.wan.password=xxx

### 设置为二级路由
uci set network.wan.proto=none              ###< 关掉wan
uci set network.lan.gateway=[一级路由ip]    ###< 网关指向一级路由
uci set network.lan.dns=[一级路由ip]        ###< dns指向一级路由
uci set dhcp.lan.ignore=1                   ###< 关掉lan的dhcp

### 无线设置
uci set wireless.@wifi-device[0].disable=0          ###< 打开无线
uci set wireless.@wifi-device[0].txpower=17         ###< 设置功率为17dbm,太高会烧坏无线模块
uci set wireless.@wifi-device[0].channel=6          ###< 设置无线信道为6
uci set wireless.@wifi-device[0].mode=ap            ###< 设置无线为ap模式
uci set wireless.@wifi-device[0].ssid=SSID
uci set wireless.@wifi-device[0].network=lan        ###< 无线链接到lan上
uci set wireless.@wifi-device[0].encryption=psk2    ###< 设置加密为WPA2-PSK
uci set wireless.@wifi-device[0].key=xxx            ###< 设置无线密码

### 应用配置
uci changes [config]                ###< 显示未保存修改(commit前)
uci commit [config]                 ###< 有点儿sqlite的味道
/etc/init.d/network restart         ###< 重启网络服务

### 该如何用uci配置某一个应用的某一个参数呢
先找到相关配置文件，比如说/etc/config/demo
$ cat /etc/config/demo
config login
    option username 'xxx'
    option password 'xxx'
    option ifname 'eth0'
    option domainname 'jphome.github.com'

config demo_list
    option ip_list '192.168.1.1'

$ uci -c /et/config show demo         ###< 显示配置文件demo的每一项的规则(名字)
demo.@login[0]=login
demo.@login[0].username=xxx
demo.@login[0].password=xxx
demo.@login[0].ifname=eth0
demo.@login[0].domainname=jphome.github.com
demo.@demo_list[0]=demo_list
demo.@demo_list[0].ip_list=192.168.1.1

然后就可以用uci get/uci set进行操作了 ^ ^

### 另外一种用法
$ xxx << EOF        ///< 将两个EOF之间的内容作为参数传给xxx,EOF可替换成任意合法字符
heredoc> param1
heredoc> param2
heredoc> EOF

那就可以将uci操作写成连贯的命令脚本了
#!/bin/sh
uci -q batch << EOF >/dev/null
add demo test_node
set demo.@test_node[0].init=0
commit demo
EOF
</pre>

#### 如何在c代码中嵌入uci进行uci get/set操作呢
还需要去调用不熟悉的网络库代码来获取这种n久不会改变一次的参数么？   `NoNoNo!`
<br/>
当然，如果会频繁get、set建议还是用c代码实现(至少用uci的api)
<br/>
思路：`popen + sed + awk`
<pre>
char result[16] = {0};
char *cmdline = NULL;
cmdline = "uci get network.lan.ipaddr";                     ###< 获取ip地址
cmdline = "ifconfig | grep HWaddr | awk '{printf $5}'";     ###< 获取mac地址
FILE fp = popen(cmdline, "r");
fgets(result, 16, fp);
pclose(fp);
</pre>


### Refs
* http://blog.csdn.net/ruiyiin/article/details/8677785
* http://www.right.com.cn/forum/thread-50257-1-1.html
* http://www.leiphone.com/diy-a-smart-router-topic-system-configuration.html

