---
title: 使用OpenWrt SDK开发ipk(helloworld.ipk)
date: '2014-03-29 16:07:24'
permalink: 
description: 
categories: 
- 技术相关

tags: 
- OpenWrt
---


### 说明
#### 几年前想编译一个helloworld.ipk,由于以下原因当年尝试失败了
<pre>
当年太水(现在还是很湿)
学校网络太操蛋(ubuntu下借脚本连闪讯,时断时序,编译个固件要纯手工6+H,想想都是泪)
太jb费时间(一个helloworld整了几天没整出来,灰心了)
</pre>

<hr/>

#### 1. 下载openwrt trunk代码
<pre>
svn co svn://svn.openwrt.org/openwrt/trunk/
</pre>

#### 2. 编译openwrt brcm63xx固件,注意勾上sdk
<pre>
make menuconfig
make V=99
</pre>

#### 3. 使用brcm63xx平台sdk编译ipk
##### 解压 sdk <code>bin/OpenWrt-SDK-xxx.tar.bz2</code>
<pre>
tar xvf OpenWrt-SDK-xxx.tar.bz2
</pre>

##### 在package路径下建立helloworld工程
<pre>
package
├── helloworld
│   ├── Makefile
│   └── src
│       ├── helloworld.c
│       └── Makefile
└── Makefile
</pre>

##### 建立staging_dir链接(交叉编译工具)
<pre>
cd sdk
ln -s ../trunk/staging_dir ./staging_dir
</pre>

##### 编译 
<pre>
make distclean
make menuconfig
make V=99
</pre>

<pre>
bin
├── brcm63xx
│   └── packages
│       ├── helloworld_1.0-1_brcm63xx.ipk
│       ├── Packages
│       └── Packages.gz
└── packages
</pre>

<hr/>

### 代码
##### package/helloworld/Makefile
<pre>
include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/package.mk

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)

PKG_NAME:=helloworld
# Version: 1.0-1
PKG_VERSION:=1.0
PKG_RELEASE:=1
PKG_MAINTAINER:=JPH <jphome98@gmail.com>
# PKG_SOURCE_URL:=

define Package/helloworld
	SECTION:=utils
	CATEGORY:=Utilities
	DEFAULT:=y
	TITLE:=Helloworld -- prints a snarky message
	# DEPENDS:=+libmath
endef

define Build/Prepare
	@echo "############## Build/Prepare"
	$(Build/Prepare/Default)
	$(CP) ./src/* $(PKG_BUILD_DIR)	
endef

define Package/helloworld/install
	@echo "############## Package/helloworld/install"
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/helloworld $(1)/usr/bin
endef

$(eval $(call BuildPackage,helloworld))
</pre>

##### package/helloworld/src/helloworld.c
<pre>
#include <stdio.h>

int main(int argc, const char *argv[])
{
    printf("hello world! \n");

    return 0;
}
</pre>

##### package/helloworld/src/Makefile
<pre>
helloworld: helloworld.o
    $(CC) helloworld.o -o helloworld

helloworld.o: helloworld.c
    $(CC) -c helloworld.c

clean:
    rm *.o helloworld
</pre>
