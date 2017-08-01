---
title: LuCI新增页面
date: '2014-08-03 19:21:14'
permalink: 
description: 
categories: 
- 技术相关

tags: 
- OpenWrt
- LuCI
---


### 说明
以luci-app-njitclient客户端学习luci新增页面
<hr/>

### 代码结构
<pre>
package
├── helloworld
├── luci-app-njitclient-master
│   ├── files
│   │   └── root
│   │       ├── etc
│   │       │   ├── config
│   │       │   │   └── njitclient
│   │       │   └── init.d
│   │       │       └── njitclient
│   │       └── usr
│   │           └── lib
│   │               └── lua
│   │                   └── luci
│   │                       ├── controller
│   │                       │   └── njitclient.lua
│   │                       └── model
│   │                           └── cbi
│   │                               └── njitclient.lua
│   ├── LICENSE
│   ├── Makefile
│   └── README.md
└── Makefile
</pre>

<hr/>

### 代码
##### package/luci-app-njitclient-master/Makefile
<pre>
include $(TOPDIR)/rules.mk

## 软件包名称，将在menuconfig和ipkg可以看到
PKG_NAME:=luci-app-njitclient
## 软件版本号
PKG_VERSION=1.0
## Makefile的版本号
PKG_RELEASE:=1
## 软件维护者
PKG_MAINTAINER:=JPH <jphome98@163.com>
## 源代码的文件名
# PKG_SOURCE
## 源代码的下载网站位置
# PKG_SOURCE_URL
# 获取代码方式可以为git、svn、cvs、hg、bzr等
# 下载方法参考$(INCLUDE_DIR)/download.mk 和 $(SCRIPT_DIR)/download.pl
#	@SF		sourceforge网站
#	@GNU	GNU网站
#	@GNOME
#	@KERNEL
## 源代码文件的效验码
# PKG_MD5SUM
## 源代码文件的解压方法
# PKG_CAT

## 软件包编译目录 [默认为$(BUILD_DIR)/$( PKG_NAME)$( PKG_VERSION)]
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)

include $(INCLUDE_DIR)/package.mk

## 编译包 定义
define Package/$(PKG_NAME)
	# 包的类型
	SECTION:=luci
	# 分类, 在menuconfig的菜单下将可以找到
	CATEGORY:=LuCI
	# 子分类 对应menuconfig中/LuCI/3. Applications/luci-app-njitclient
	SUBMENU:=3. Applications
	# 软件包的简短描述
	TITLE:=NJIT 802.1X Client for LuCI
	# control中的 Architecture 字段, 默认为当前编译器的交叉编译环境平台
	PKGARCH:=all
	# URL 				软件包的下载地址
	# MAINTAINER		维护者
	# DEPENDS 			与其他软件的依赖, 即如编译或安装需要其他软件时需要说明 DEPENDS:=+libmath +libzlib
endef

## 软件包的详细描述, control中的 Description 字段, 默认为TITLE
define Package/$(PKG_NAME)/description
	This package contains LuCI configuration pages for njit8021xclient.
endef

## 编译准备 创建文件夹、拷贝源文件
define Build/Prepare
endef

## 针对需要配置的软件包 Automake中./configure
define Build/Configure
endef

## 编译方法 没特殊说明不定义 使用默认编译方法
# $(MAKE) -C $(PKG_BUILD_DIR) \
# 	$(TARGET_CONFIGURE_OPTS) CFLAGS="$(TARGET_CFLAGS) -I$(LINUX_DIR)/include"
define Build/Compile
endef

## 软件包的安装方法		$(1) -> 嵌入系统的镜像目录 /
define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_DIR) $(1)/usr/lib/lua/luci/model/cbi
	$(INSTALL_DIR) $(1)/usr/lib/lua/luci/controller
	
	$(INSTALL_CONF) ./files/root/etc/config/njitclient $(1)/etc/config/njitclient
	$(INSTALL_BIN) ./files/root/etc/init.d/njitclient $(1)/etc/init.d/njitclient
	$(INSTALL_DATA) ./files/root/usr/lib/lua/luci/model/cbi/njitclient.lua $(1)/usr/lib/lua/luci/model/cbi/njitclient.lua
	$(INSTALL_DATA) ./files/root/usr/lib/lua/luci/controller/njitclient.lua $(1)/usr/lib/lua/luci/controller/njitclient.lua
endef

## 软件包安装前处理方法, 使用脚本语言
define Package/$(PKG_NAME)/preinst
	#!/bin/sh
	echo "###$(PKG_MAINTAINER): To install $(PKG_NAME)"
endef

## 软件包安装后处理方法, 使用脚本语言
define Package/$(PKG_NAME)/postinst
	#!/bin/sh
	echo "###$(PKG_MAINTAINER): $(PKG_NAME) install succeed"
endef

## 软件包删除前处理方法, 使用脚本语言
define Package/$(PKG_NAME)/prerm
	#!/bin/sh
	echo "###$(PKG_MAINTAINER): To remove $(PKG_NAME)"
endef

## 软件包删除后处理方法, 使用脚本语言
define Package/$(PKG_NAME)/postrm
	#!/bin/sh
	echo "###$(PKG_MAINTAINER): $(PKG_NAME) remove succeed"
endef

## 使用eval函数实现各种定义
$(eval $(call BuildPackage,$(PKG_NAME))
</pre>

##### package/luci-app-njitclient-master/files/root/usr/lua/luci/controller/njitclient.lua
<pre>
--[[ 模块入口 ]]

-- 程序模块名	/usr/lib/lua/luci/controller/njitclient.lua
module("luci.controller.njitclient", package.seeall)

function index()
	-- 添加一个新的模块(Page)入口
	-- entry(路径, 调用目标, _("显示名称"), 显示顺序)
	-- 路径 以字符串数组给定 {"admin", "network", "njitclient"} -> http://ip/cgi-bin/luci/admin/network/njitclient
	-- 调用目标：
		-- 执行指定方法（Action）：call("function_name")
		-- 访问指定页面（Views）：template("myapp/mymodule")	访问/usr/lib/lua/luci/view/myapp/mymodule.htm
		-- 调用CBI(Configuration Binding Interface) Module：cbi("myapp/mymodule")	调用/usr/lib/lua/luci/model/cbi/myapp/mymodule.lua
		-- 重定向：alias("admin", "status", "overview")
	entry({"admin", "network", "njitclient"}, cbi("njitclient"), _("NJIT Client"), 100)
	end
</pre>

##### package/luci-app-njitclient-master/files/root/usr/lua/luci/model/cbi/njitclient.lua
<pre>
--[[ 配置模块实际的代码 ]]

require("luci.sys")

-- 映射与存储文件的关系
-- m = Map("配置文件文件名", "配置页面标题", "配置页面说明")
	-- 配置文件文件名：配置文件存储的文件名，不包含路径 /etc/config/xxx
m = Map("njitclient", translate("NJIT Client"), translate("Configure NJIT 802.11x client."))

-- 配置文件中对应的section
s = m:section(TypedSection, "login", "")
-- 不允许增加或删除
s.addremove = false
-- 不显示Section的名称
s.anonymous = true

-- 配置文件中section的交互(创建Option)
	-- Value（文本框）
	-- ListValue（下拉框）
	-- Flag（选择框）
enable = s:option(Flag, "enable", translate("Enable"))
name = s:option(Value, "username", translate("Username"))
pass = s:option(Value, "password", translate("Password"))
pass.password = true
domain = s:option(Value, "domain", translate("Domain"))

ifname = s:option(ListValue, "ifname", translate("Interfaces"))
for k, v in ipairs(luci.sys.net.devices()) do
	if v ~= "lo" then
		ifname:value(v)
	end
end

-- 判断是否点击了“应用”按钮
local apply = luci.http.formvalue("cbi.apply")
if apply then
	io.popen("/etc/init.d/njitclient restart")
end

return m
</pre>

##### package/luci-app-njitclient-master/files/root/etc/init.d/njitclient
<pre>
#!/bin/sh /etc/rc.common
START=50

run_njit()
{
	local enable
	# 获取变量值
	# config_get_bool 变量名 Section名 Section参数名
	config_get_bool enable $1 enable
	
	if [ $enable ]; then
		local username
		local password
		local domain
		local ifname
		
		# 获取变量值
		# config_get 变量名 Section名 Section参数名
		config_get username $1 username
		config_get password $1 password
		config_get domain $1 domain
		config_get ifname $1 ifname
		
		if [ "$domain" != "" ]; then
			njit-client $username@$domain $password $ifname &
		else
			njit-client $username $password $ifname &
		fi
		
		echo "NJIT Client has started."
	fi
}

start()
{
	# 读取配置文件
	config_load njitclient
	
	# 遍历配置文件中的Section
	# config_foreach 遍历函数名 Section类型
	config_foreach run_njit login
}

stop()
{
	killall njit-client
	killall udhcpc
	
	echo "NJIT Client has stoped."
}
</pre>

##### package/luci-app-njitclient-master/files/root/etc/init.d/njitclient
<pre>
config login
	option username ''
	option password ''
	option ifname 'eth0'
	option domain ''
</pre>

<hr/>

### Refs
* http://chaochaoblog.com/archives/359
* http://www.7forz.com/1973/
* http://www.cnblogs.com/andy-ahz/p/3722538.html
