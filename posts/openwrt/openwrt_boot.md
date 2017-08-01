---
title: OpenWrt启动脚本分析
date: '2014-08-13 23:38:46'
permalink: 
description: 
categories: 
- 技术相关

tags: 
- OpenWrt
---

### 说明
OpenWrt是通过一系列shell脚本进行启动流程的组织，本文介绍启动流程。如果想详细了解启动的过程，则需要仔细走读脚本文件。

<hr/>

<pre>
1. 在make menuconfig 选择target平台 Broadcom BCM947xx/953xx [2.4]

2. linux内核的配置文件由下面两个文件组成
target/linux/generic-2.4/config-default
target/linux/brcm-2.4/config-default

3. 在配置文件中可以看到
CONFIG_CMDLINE="root=/dev/mtdblock2 rootfstype=squashfs,jffs2
init=/etc/preinit noinitrd console=ttyS0,115200"
因此，linux内核启动后，首先运行/etc/preinit脚本

4. preinit脚本位置在
package/base-files/files/etc/preinit

5. preinit脚本是一系列脚本的入口，这一系列脚本放在下面的目录：
package/base-files/files/lib/preinit
target/linux/brcm-2.4/base-files/lib/preinit
编译完成后，会统一放在rootfs的/lib/preinit目录下，
03_init_hotplug_failsafe_brcm 40_init_shm
05_failsafe_config_switch_brcm 40_mount_devpts
05_init_interfaces_brcm 40_mount_jffs2
05_mount_skip 40_run_failsafe_hook
05_set_failsafe_switch_brcm 41_merge_overlay_hooks
10_check_for_mtd 50_choose_console
10_essential_fs 50_indicate_regular_preinit
10_indicate_failsafe 60_init_hotplug
10_indicate_preinit 70_initramfs_test
15_mount_proc_brcm 70_pivot_jffs2_root
15_set_preinit_interface_brcm 80_mount_root
20_check_jffs2_ready 90_init_console
20_device_fs_mount 90_mount_no_jffs2
20_failsafe_net_echo 90_restore_config
20_failsafe_set_boot_wait_brcm 99_10_failsafe_login
30_device_fs_daemons 99_10_mount_no_mtd
30_failsafe_wait 99_10_run_init
由于脚本众多，因此openwrt的设计者将这些脚本分成下面几类：
preinit_essential
preinit_main
failsafe
initramfs
preinit_mount_root
每一类函数按照脚本的开头数字的顺序运行。

6. preinit则执行下面的两类脚本
boot_run_hook preinit_essential
boot_run_hook preinit_main

7. preinit执行的最后一个脚本为99_10_run_init，运行
exec env - PATH=$pi_init_path $pi_init_env $pi_init_cmd
pi_init_cmd为
pi_init_cmd="/sbin/init"
因此开始运行busybox的init命令

8. busybox的init命令执行inittab的脚本，该脚本来自
package/base-files/files/etc/inittab
::sysinit:/etc/init.d/rcS S boot
::shutdown:/etc/init.d/rcS K stop
tts/0::askfirst:/bin/ash --login
ttyS0::askfirst:/bin/ash --login
tty1::askfirst:/bin/ash --login
sysinit为系统初始化运行的 /etc/init.d/rcS S boot脚本
shutdown为系统重启或关机运行的脚本
tty开头的是，如果用户通过串口或者telnet登录，则运行/bin/ash --login
askfirst和respawn相同，只是在运行前提示"Please press Enter to activate
this console."

9. 当前启动转到运行 /etc/init.d/rcS S boot，该脚本来自
package/base-files/files/etc/init.d/rcS
和preinit类似，rcS也是一系列脚本的入口，其运行/etc/rc.d目录下S开头的的所
有脚本(如果运行rcS K stop，则运行K开头的所有脚本)
K50dropbear S02nvram S40network S50dropbear S96led
K90network S05netconfig S41wmacfixup S50telnet S97watchdog
K98boot S10boot S45firewall S60dnsmasq S98sysntpd
K99umount S39usb S50cron S95done S99sysctl
上面的脚本文件来自：
package/base-files/files/etc/init.d
target/linux/brcm-2.4/base-files/etc/init.d
还有一些脚本来自各个模块，在install时拷贝到rootfs，比如dropbear模块
package/dropbear/files/dropbear.init
这些脚本先拷贝到/etc/init.d下，然后通过/etc/rc.common脚本，将init.d的脚
本链接到/etc/rc.d目录下，并且根据 这些脚本中的START和STOP的关键字，添加
K${STOP}和S${START}的前缀，这样就决定了脚本的先后的运行次序。

10.可以看出，openwrt的启动主要是两个阶段，preinit主要是完成系统的初始化
（如文件系统的准备、模块的加载），rcS主要依次 启动各个模块。

附：脚本走读的一些技巧
a. rootfs目录在build_dir/target-mipsel_uClibc-0.9.30.1/root-brcm-2.4，可以直接在该目录下走读shell脚本。
b. openwrt的shell脚本比较复杂，因此看脚本时可以通过添加 set -x和echo等命令，直接看shell脚本的结果，而不要花太多的时间硬看脚本，主要是理解其主要的意思和设计思路。
</pre>

<hr/>

### Refs
* http://blog.chinaunix.net/uid-26598889-id-3060545.html

