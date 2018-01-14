---
layout: post
title: 内存取证工具volatility的使用简介
category: 技术
tags: 取证
keywords:
description:
---

### 1.volatility简介

volatility 【1】框架是一个完全开放的工具集合，在GNU通用许可证下用Python实现，用于从易失性存储器（RAM）样本总提取数字镜像。提取技术完全独立于被取证的系统而执行，但可以查看到系统运行时的状态信息。该框架旨在向人们介绍从内存样本中提取的数字镜像相关的技术，并为进一步研究该领域提供一个平台。

### 2. 相关使用和测试资源

* volatility 工具，https://github.com/volatilityfoundation/volatility

* 操作系统：Kali 2017 32bit 

* 测试镜像（Compromised_Server-memdump.img.zip，通过memdump获得的32位debian内存镜像），链接: https://pan.baidu.com/s/1pNiKoUR 密码: bhvn

### 3. 使用步骤和方法

* 所有操作，都在Kali 2017 32bit下进行 

##### 3.1 配置volatility

（1）从https://github.com/volatilityfoundation/profiles中下载linux的profile模版，主要是加入Debian x86镜像到/usr/lib/python2.7/dist-packages/volatility/plugins/overlays/linux目录下；

（2）运行volatility，获取profile信息

```
root@kali:~/Downloads# volatility imageinfo -f Compromised_Server-memdump.img Volatility Foundation Volatility Framework 2.6 
INFO : volatility.debug : Determining profile based on KDBG search… 
Suggested Profile(s) : No suggestion (Instantiated with LinuxDebian5010x86) 
AS Layer1 : IA32PagedMemory (Kernel AS) 
AS Layer2 : FileAddressSpace (/root/Downloads/Compromised_Server-memdump.img) 
PAE type : No PAE 
DTB : 0x3bc000L 
root@kali:~/Downloads#
```

（3）查看volatility支持的命令

```
root@kali:~/Downloads# volatility --info|grep linux 
Volatility Foundation Volatility Framework 2.6 
linux_apihooks - Checks for userland apihooks 
linux_arp - Print the ARP table 
linux_aslr_shift - Automatically detect the Linux ASLR shift 
linux_banner - Prints the Linux banner information 
linux_bash - Recover bash history from bash process memory 
linux_bash_env - Recover a process' dynamic environment variables 
linux_bash_hash - Recover bash hash table from bash process memory 
linux_check_afinfo - Verifies the operation function pointers of network
```

（4）运行volatility命令

```
root@kali:~/Downloads# volatility linux_psaux --profile=LinuxDebian5010x86 -f Compromised_Server-memdump.img 
Volatility Foundation Volatility Framework 2.6 
Pid Uid Gid Arguments 
1 0 0 init [2] 
2 0 0 [kthreadd] 
3 0 0 [migration/0] 
4 0 0 [ksoftirqd/0] 
5 0 0 [watchdog/0] 
6 0 0 [events/0] 
7 0 0 [khelper] 
39 0 0 [kblockd/0] 
41 0 0 [kacpid] 
42 0 0 [kacpi_notify] 
86 0 0 [kseriod] 
123 0 0 [pdflush] 
124 0 0 [pdflush] 
125 0 0 [kswapd0] 
126 0 0 [aio/0] 
581 0 0 [ksuspend_usbd] 
582 0 0 [khubd] 
594 0 0 [ata/0] 
595 0 0 [ata_aux] 
634 0 0 [scsi_eh_0] 
700 0 0 [kjournald] 
776 0 0 udevd --daemon 
1110 0 0 [kpsmoused] 
1429 1 1 /sbin/portmap 
1441 102 0 /sbin/rpc.statd 
1624 0 0 dhclient3 -pf /var/run/dhclient.eth0.pid -lf /var/lib/dhcp3/dhclient.eth0.leases eth0 
1661 0 0 /usr/sbin/rsyslogd -c3 
1672 0 0 /usr/sbin/acpid 
1687 0 0 /usr/sbin/sshd 
1942 101 103 /usr/sbin/exim4 -bd -q30m 
1973 0 0 /usr/sbin/cron 
1990 0 0 /bin/login -- 
1992 0 0 /sbin/getty 38400 tty2 
1994 0 0 /sbin/getty 38400 tty3 
1996 0 0 /sbin/getty 38400 tty4 
1998 0 0 /sbin/getty 38400 tty5 
2000 0 0 /sbin/getty 38400 tty6

```

##### 3.2 详细操作

（1）服务器中运行了哪种系统（操作系统，CPU）

```
root@kali:~/Downloads# volatility linux_dmesg --profile=LinuxDebian5010x86 -f Compromised_Server-memdump.img 
Volatility Foundation Volatility Framework 2.6 
<6>[ 0.000000] Initializing cgroup subsys cpuset 
<6>[ 0.000000] Initializing cgroup subsys cpu 
<5>[ 0.000000] Linux version 2.6.26-2-686 (Debian 2.6.26-26lenny1) (dannf@debian.org) (gcc version 4.1.3 20080704 (prerelease) (Debian 4.1.2-25)) #1 SMP Thu Nov 25 01:53:57 UTC 2010
```

可以看到的相关信息如下：

```
Distro: Debian 2.6.26-26lenny1

Kernel: 
Linux version 2.6.26-2-686 (Debian 2.6.26-26lenny1) (dannf@debian.org) (gcc version 4.1.3 20080704 (prerelease) 
(Debian 4.1.2-25)) #1 SMP Thu Nov 25 01:53:57 UTC 2010

Cpu: 
Processor Vendor Model 
0 GenuineIntel Intel(R) Core(TM)2 CPU T7200 @ 2.00GHz

Memory: 
249924k/262080k available (1771k kernel code, 11584k reserved, 749k data, 244k init, 0k highmem)

Timezone: 
Europe/Paris (CET)

Disk: 
1 Partition ext3 (sda1) 
1 Swap (sda5)

```

（2）服务器中运行了哪些进程？

```
root@kali:~/Downloads# volatility linux_pslist --profile=LinuxDebian5010x86 -f Compromised_Server-memdump.img 
Volatility Foundation Volatility Framework 2.6 
Offset Name Pid PPid Uid Gid DTB Start Time

0xcf42f900 init 1 0 0 0 0x0f4b8000 2011-02-06 12:04:09 UTC+0000 
0xcf42f4e0 kthreadd 2 0 0 0 ---------- 2011-02-06 12:04:09 UTC+0000 
0xcf42f0c0 migration/0 3 2 0 0 ---------- 2011-02-06 12:04:09 UTC+0000 
0xcf42eca0 ksoftirqd/0 4 2 0 0 ---------- 2011-02-06 12:04:09 UTC+0000 
0xcf42e880 watchdog/0 5 2 0 0 ---------- 2011-02-06 12:04:09 UTC+0000 
0xcf42e460 events/0 6 2 0 0 ---------- 2011-02-06 12:04:09 UTC+0000 
0xcf42e040 khelper 7 2 0 0 ---------- 2011-02-06 12:04:09 UTC+0000 
0xcf4a1a40 kblockd/0 39 2 0 0 ---------- 2011-02-06 12:04:09 UTC+0000 
0xcf4a1200 kacpid 41 2 0 0 ---------- 2011-02-06 12:04:09 UTC+0000 
0xcf45d140 kacpi_notify 42 2 0 0 ---------- 2011-02-06 12:04:09 UTC+0000 
0xcf46c940 kseriod 86 2 0 0 ---------- 2011-02-06 12:04:09 UTC+0000 
0xcf43f100 pdflush 123 2 0 0 ---------- 2011-02-06 12:04:10 UTC+0000 
0xcf45d980 pdflush 124 2 0 0 ---------- 2011-02-06 12:04:10 UTC+0000 
0xcf45d560 kswapd0 125 2 0 0 ---------- 2011-02-06 12:04:10 UTC+0000 
0xcf43f520 aio/0 126 2 0 0 ---------- 2011-02-06 12:04:10 UTC+0000 
0xcf45c4e0 ksuspend_usbd 581 2 0 0 ---------- 2011-02-06 12:04:14 UTC+0000 
0xcf48d1c0 khubd 582 2 0 0 ---------- 2011-02-06 12:04:14 UTC+0000 
0xcf46d9c0 ata/0 594 2 0 0 ---------- 2011-02-06 12:04:15 UTC+0000 
0xcf802a00 ata_aux 595 2 0 0 ---------- 2011-02-06 12:04:15 UTC+0000 
0xcf43e080 scsi_eh_0 634 2 0 0 ---------- 2011-02-06 12:04:17 UTC+0000 
0xcf45c0c0 kjournald 700 2 0 0 ---------- 2011-02-06 12:04:18 UTC+0000 
0xcf46d5a0 udevd 776 1 0 0 0x0f5b2000 2011-02-06 12:04:21 UTC+0000 
0xce978620 kpsmoused 1110 2 0 0 ---------- 2011-02-06 12:04:27 UTC+0000 
0xce9796a0 portmap 1429 1 1 1 0x0eddf000 2011-02-06 12:04:35 UTC+0000 
0xce973b00 rpc.statd 1441 1 102 0 0x0f8b3000 2011-02-06 12:04:35 UTC+0000 
0xcf45c900 dhclient3 1624 1 0 0 0x0ec3d000 2011-02-06 12:04:39 UTC+0000 
0xce972660 rsyslogd 1661 1 0 0 0x0e7ed000 2011-02-06 12:04:40 UTC+0000 
0xcf43ece0 acpid 1672 1 0 0 0x0f8a8000 2011-02-06 12:04:40 UTC+0000 
0xce979ac0 sshd 1687 1 0 0 0x0fa65000 2011-02-06 12:04:41 UTC+0000 
0xcf45cd20 exim4 1942 1 101 103 0x0e7bc000 2011-02-06 12:04:44 UTC+0000 
0xcf803a80 cron 1973 1 0 0 0x0f815000 2011-02-06 12:04:45 UTC+0000 
0xcfaad720 login 1990 1 0 0 0x0eecf000 2011-02-06 12:04:45 UTC+0000 
0xcf48c560 getty 1992 1 0 0 0x0ea31000 2011-02-06 12:04:45 UTC+0000 
0xcf803240 getty 1994 1 0 0 0x0f671000 2011-02-06 12:04:45 UTC+0000 
0xcf4a1620 getty 1996 1 0 0 0x0f838000 2011-02-06 12:04:45 UTC+0000 
0xcf46cd60 getty 1998 1 0 0 0x0f83d000 2011-02-06 12:04:45 UTC+0000 
0xcf4a0180 getty 2000 1 0 0 0x0e89e000 2011-02-06 12:04:45 UTC+0000 
0xcf8021c0 bash 2042 1990 0 0 0x0eecc000 2011-02-06 14:04:38 UTC+0000 
0xcfaacee0 sh 2065 1 0 0 0x0f517000 2011-02-06 14:07:15 UTC+0000 
0xcfaac280 memdump 2168 2042 0 0 0x08088000 2011-02-06 14:42:27 UTC+0000 
0xcf43e8c0 nc 2169 2042 0 0 0x08084000 2011-02-06 14:42:27 UTC+0000

```

（3）攻击者和攻击目标的IP？

查看网络情况 


```

root@kali:~/Downloads# volatility linux_netstat --profile=LinuxDebian5010x86 -f Compromised_Server-memdump.img 
Volatility Foundation Volatility Framework 2.6 
UNIX 2190 udevd/776 
UDP 0.0.0.0 : 111 0.0.0.0 : 0 portmap/1429 
TCP 0.0.0.0 : 111 0.0.0.0 : 0 LISTEN portmap/1429 
UDP 0.0.0.0 : 769 0.0.0.0 : 0 rpc.statd/1441 
UDP 0.0.0.0 :38921 0.0.0.0 : 0 r

```



可以得出：

192.168.56.1 0a:00:27:00:00:00 
192.168.56.101 08:00:27:28:5a:cc


### 注意事项

> 从不同版本系统中提取的内存镜像，支持下相同位数的版本软件进行分析，即32位Windows 7的版本内存镜像分析需要在32位Linux系统下使用volatility进行分析；

> 不同版本的volatility所支持的命令的表达不同，需要通过```volatility --info```进行查看；

### 参考

【1】Volatility Framework - Volatile memory extraction utility framework，https://github.com/volatilityfoundation/volatility