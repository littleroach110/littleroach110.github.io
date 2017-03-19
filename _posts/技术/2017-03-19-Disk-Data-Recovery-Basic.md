---
layout: post
title: 硬盘数据恢复的流程和方法（基本篇:）
category: 技术
tags: 数据恢复
keywords:
description:
---

### 1. 硬盘数据恢复的场景及目标

* 针对硬盘/U盘出现故障，无法打开硬盘/U盘，但该盘符依然可读，需要进行数据恢复的情况；

* 数据取证的场景，考虑到原始数据的证据效力，需要对整个磁盘进行数据备份后，对备份文件完成取证操作；

### 2. 恢复使用的软件

* WinHex， 百度网盘下载地址：https://pan.baidu.com/s/1bWMxPW：

* DiskGenius， 百度网盘下载地址：https://pan.baidu.com/s/1dEXwl4t

### 3. 硬盘恢复步骤

##### 3.1 使用Winhex对硬盘/磁盘进行制作镜像文件的操作

（1）打开winhex，选择“工具”-> "磁盘工具"->"克隆磁盘"。

![Data Forensic 01]({{site.CDN_PATH}}/public/image/20170319-Recovery-The-File-01.png)

（2）选择源磁盘和保存磁盘镜像文件的目录。

![Data Forensic 02]({{site.CDN_PATH}}/public/image/20170319-Recovery-The-File-02.png)

（3）选择磁盘时，可以选择逻辑磁盘或物理磁盘。在本次测试中，选择物理磁盘。

![Data Forensic 03]({{site.CDN_PATH}}/public/image/20170319-Recovery-The-File-03.png)

（4）选择完成后，点击“OK”，开始对磁盘进行克隆。

![Data Forensic 04]({{site.CDN_PATH}}/public/image/20170319-Recovery-The-File-04.png)

（5）对磁盘进行克隆完成。

![Data Forensic 05]({{site.CDN_PATH}}/public/image/20170319-Recovery-The-File-05.png)

（6）选择"专家"->"映射文件为磁盘"。

![Data Forensic 06]({{site.CDN_PATH}}/public/image/20170319-Recovery-The-File-06.png)

（7） 得到镜像文件映射为磁盘的样式。

![Data Forensic 07]({{site.CDN_PATH}}/public/image/20170319-Recovery-The-File-07.png)

（8）点击“Partition1”，进入加载该镜像文件，可以得到的目录结构。其中，可以看到一些已删除的文件及目录（可以用于数据恢复）。

![Data Forensic 08]({{site.CDN_PATH}}/public/image/20170319-Recovery-The-File-08.png)
 
 ##### 3.2 使用DiskGenius对硬盘/磁盘进行制作镜像文件的操作

（1）打开DiskGenius，可以看到主机中基本的硬盘加载情况。

![Data Forensic 09]({{site.CDN_PATH}}/public/image/20170319-Recovery-The-File-09.png)
 
（2）选择“硬盘”->"打开虚拟硬盘文件"，打开刚刚使用winhex进行备份的镜像文件。

![Data Forensic 10]({{site.CDN_PATH}}/public/image/20170319-Recovery-The-File-10.png)

（3）在标识的图中，可以看到镜像文件的目录及文件情况，包括已删除的一些目录和文件，可以选择性的对它们进行恢复操作。

![Data Forensic 11]({{site.CDN_PATH}}/public/image/20170319-Recovery-The-File-11.png)
