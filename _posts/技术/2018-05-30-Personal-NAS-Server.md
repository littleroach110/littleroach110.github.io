---
layout: post
title: 基于Gen10和黑群晖的个人NAS服务器构建（安装篇）
category: 技术
tags: DIY
keywords:
description:
---

是否还因为存放岛国小姐姐的硬盘房间太小而无奈？

![Personal NAS Server 01]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER01.png)

是否还在为网盘里珍藏的动作爱情片变成了十秒教育视频而苦恼？

![Personal NAS Server 02]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER02.png)

﻿个人NAS为各位宅男腐女们送来了福音！！！


﻿============================ 这是正经的分界线 ============================


NAS，Network Attached Storage，网络附属存储，基于标准网络协议实现数据传输，为网络中的Windows / Linux / Mac OS 等各种不同操作系统的计算机提供文件共享和数据备份【1】。

本着省钱实用的目的，对构建个人NAS服务器的过程和过程中踩过的坑进行了梳理。

###  一、构建目标和需求

构建个人NAS服务器时，会重点关注一下几方面的问题：

#####  1. 成本低

在大部分人选择NAS时，成本是首要关注的问题。选择大型商用的NAS服务器时，动辄需要上万元；选择网盘时，私密数据交给别人保管，反正我是不放心的；选择云服务器时，费用也不便宜，而且网速也是瓶颈。

千元级别的硬件成本，以及较低廉的家庭用电费用，使得构建个人NAS服务器在成本方面，具有较大的优势。

##### ﻿2. 性能稳定

个人搭建NAS服务器，意味着前期准备和后期运维工作，都需要自己亲力亲为。选择性能稳定的硬件和软件，显得尤为必要，可以大大减轻整个过程中的工作量，少踩很多意想不到的坑。

##### ﻿3. 功能丰富

既然搭建了一台NAS服务，如果只是将其用来存储数据，那会显得很浪费。该服务器最好还可以用于开发（如写个爬虫，搭个网站等）、家庭娱乐用（电视和投影仪可以直接连上，用于电影、电视剧放映）、手机也能直接连上进行处理和操作。

##### ﻿4. 影响小

由于服务器放在家里，因此服务器的声音要小；另外，服务器的电耗要低（100W以内最合适）。

###  二、方案选择

##### ﻿1. 服务器

最终选择作为NAS系统的服务器为HP Gen10，一方面是前代机型Gen8攒下的超高人气和口碑，另一方面HP大厂出品，值得信赖，且主机配置相对较高，体型迷你。

![Personal NAS Server 03]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER03.png)

![Personal NAS Server 03.5]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER03.5.png)

根据Gen10的官方文档，Gen10官方文档【2】，Gen10支持4个SATA硬盘，单块硬盘最大支持4TB；另外，顶部预留的光驱位还可以外接一个硬盘（部署系统用）。

![Personal NAS Server 04]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER04.png)

放弃选择原装群晖的原因是价格较高，京东四盘位群晖NAS最低都要近3000元左右，而且配置较低，用途单一。

![Personal NAS Server 05]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER05.png)

放弃选择淘宝攒机器的原因是性能不稳定，而且性价比优势不明显。

在功耗方面，有人进行了功耗测试【3】，待机状态大概1.8W，开机过程36W，性能测试时在50W左右。算下来，一天最多两度电，合一块钱左右。

![Personal NAS Server 06]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER06.png)

![Personal NAS Server 07]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER07.png)

![Personal NAS Server 08]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER08.png)

##### ﻿2. 软件

在软件方面，选择（黑）群晖系统，一方面，群晖系统在国内NAS中是做的比较出色的系统，系统整体稳定，用户交互设计优秀，支持的套件丰富，可以充分支持数据存储、家庭娱乐、开发等相关场景和应用。

放弃Win系统，主要是考虑Win系统作为服务器的稳定性；放弃Ubuntu系统，主要是考虑Ubuntu作为个人NAS，可玩性弱。

![Personal NAS Server 09]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER09.png)

![Personal NAS Server 10]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER10.png)

值得一提的是，群晖系统支持Java、Python、Perl、Ruby、Node.js、PHP等开发语言，基本满足各种开发场景的需求。

![Personal NAS Server 11]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER11.png)

##### ﻿3. 采购方案

(1) HP Gen10

目前HP Gen10在国内还没有上市，可以选择Amazon海淘的方式进行购买，Gen10的Amazon链接为：https://www.amazon.cn/gp/product/B072X2YJ2N/。

值得一提的是：选择Amazon Prime会员试用可以免运费，最近Gen10涨价了，本人当初买的时候免运费只花了2479元。

![Personal NAS Server 12]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER12.png)

![Personal NAS Server 13]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER13.png)

(2) 硬盘

NAS系统中，硬盘可分为部署NAS系统硬盘和存储硬盘。

部署NAS系统硬盘，推荐使用SSD硬盘，保证较快的启动速度。目前120GB的SSD硬盘性价比最高，去京东上随便挑。

本人选择的是闪迪120GB SSD硬盘，购买链接：https://item.jd.com/1398976.html

![Personal NAS Server 14]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER14.png)

存储硬盘选择传统机械硬盘，也是选择性价比最高的就行，目前3TB的机械硬盘性价比最高。

本人选择的硬盘是希捷酷鱼系列 3TB机械硬盘，购买链接：https://item.jd.com/3355984.html

![Personal NAS Server 15]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER15.png)

(3) UPS应急电源

应急电源的作用是为了防止突然断电导致的硬盘损坏，保护硬盘，选择600W的KLS应急电源，性价比高（半块硬盘的价格），够用。

本人选择京东购买，购买链接：https://item.jd.com/1574252549.html

![Personal NAS Server 16]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER16.png)

(4) 小4P公转SATA线

由于Gen10光驱位用于连接SSD固态硬盘，安装群晖NAS系统，外接一根小4P公转SATA线，才可保证SSD固态硬盘可用。

本人选择淘宝购买，购买链接：https://item.taobao.com/item.htm?spm=a1z09.2.0.0.56ec2e8dXtjR5c&id=523145020728&_u=tc7g7e15dce

![Personal NAS Server 17]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER17.png)

### ﻿三、部署安装

##### ﻿1. 硬件准备

(1) HP Gen10

(2) 应急电源

(3) 1块SSD固态盘+4块SATA机械硬盘+小4P公转SATA线

(4) DP线或VGA线

(5) 显示器+键鼠

(6) U盘

(7) Gen10 国标电源线或电源转换头（默认是欧标和德标的电源线）

##### ﻿2. 软件准备

(1)﻿WePE

(2)﻿osfmount

(3)﻿Notepad++

(4) IMG写盘工具

(5)﻿SynologyAssistantSetup晖群软件助手

(6)﻿SN随机生产网址和MAC计算工具

(7)﻿群晖pat文件

(8)﻿群晖SSD引导文件

【完整群晖安装包集合下载： 链接：https://pan.baidu.com/s/1u3kpcMxFC62_jEct24qN9w 密码：8lxb】

##### ﻿3. 制作启动U盘【4】

(1) 运行WePE_64_V2.0.exe，将WinPE 工具箱安装到U盘。

![Personal NAS Server 18]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER18.png)

![Personal NAS Server 19]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER19.png)

(2) 利用MAC计算文件夹中的sn及Synology-mod-new-w，计算出序列号及MAC地址。

sn文件中是一个网址：http://www.wifihell.com/serial_generator/serial_generator_new.html

打开后，选择DS3617xs，生成序列号。

![Personal NAS Server 20]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER20.png)

使用EXCEL打开文件Synology-mod-new-w，启用宏，将生成的序列号粘贴到EXCEL的Serial number处，点击任意空白处，直接跳转到Synology Mac的工作表中，记录下MAC地址。

![Personal NAS Server 21]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER21.png)

![Personal NAS Server 22]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER22.png)

(3) 安装软件中的osfmount，并挂载synoboot.img镜像中的partition 0，注意：synoboot.img必须放在全为英文的目录路径下。另外，不要勾选Read-only drive。

![Personal NAS Server 23]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER23.png)

![Personal NAS Server 24]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER24.png)

![Personal NAS Server 25]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER25.png)

(4)﻿修改grub.cfg文件，使用在（2）中生成的SN和EXCEL计算出的MAC值，在Notepad++编辑grub.cfg文件。

![Personal NAS Server 26]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER26.png)

修改完grub.cfg文件后保存文件，然后基础synoboot.img镜像挂载。

![Personal NAS Server 27]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER27.png)

(5)﻿将修改好的synoboot.img以及IMG写盘工具拷贝到制作好的WinPE U盘中，然后将U盘插入到准备安装的群辉系统的设备上，开机进入WinPE。

(6)﻿打开IMG写盘工具，选择要写入的SSD硬盘（选择物理磁盘，Physical Disk），写入镜像。

![Personal NAS Server 28]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER28.png)

![Personal NAS Server 29]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER29.png)

![Personal NAS Server 30]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER30.png)

(7) 到此，群晖引导镜像已经成功写入SSD硬盘，从Win PE中关闭设备，拔掉U盘，重新开机，开机选择SSD硬盘进行启动。

注意：在选择SSD硬盘启动后，在屏幕黑屏时，不停按键盘的上、下方向键，选择引导系统的第二项Reinstall，否则，默认从引导系统的第一项进入系统，可能会出现群辉助手一直搜索不到设备的情况。

![Personal NAS Server 31]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER31.png)

![Personal NAS Server 32]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER32.png)

到此，SSD引导系统已经成功启动。

#####  4.﻿安装群晖系统

在提示Booting the kernel后等个1-2分钟再浏览器里面输入http://find.synology.com/ ，搜索DSM，如果没有找到，那么使用SynologyAssistant查找【5】。

(1)﻿根据提示，选择手动安装压缩包中的pat文件。

![Personal NAS Server 33]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER33.png)

![Personal NAS Server 34]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER34.png)

![Personal NAS Server 35]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER35.png)

![Personal NAS Server 36]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER36.png)

![Personal NAS Server 37]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER37.png)

![Personal NAS Server 38]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER38.png)

(2) 电脑重启后，对系统进行基本的设置

![Personal NAS Server 39]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER39.png)

(3)﻿在搜索DSM，如果没有找到，那么使用SynologyAssistant查找（找到并顺利安装，忽略该步骤）。

![Personal NAS Server 40]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER40.png)

![Personal NAS Server 41]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER41.png)

通过群晖助手安装DSM系统

![Personal NAS Server 42]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER42.png)

![Personal NAS Server 43]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER43.png)

设置管理员账号及密码

![Personal NAS Server 44]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER44.png)

网络设置

![Personal NAS Server 45]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER45.png)

安装群晖系统

![Personal NAS Server 46]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER46.png)

安装完成

![Personal NAS Server 47]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER47.png)

(4)﻿跳过设置QuickConnect，并进入系统。

![Personal NAS Server 48]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER48.png)

![Personal NAS Server 49]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER49.png)

![Personal NAS Server 50]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER50.png)

![Personal NAS Server 51]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER51.png)

注意：虽然本人测试的压缩包中的晖群DSM系统，在版本升级后，没有出现异常情况，但是不推荐进行版本升级，以避免出现系统不稳定等异常情况。

![Personal NAS Server 52]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER52.png)

###  异常情况处置

#####  1. 错误代码38

![Personal NAS Server 53]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER53.png)

解决方法：

服务器中，除了系统盘以外，还需要插入至少一块SATA硬盘。

#####  2. 错误代码21

![Personal NAS Server 54]({{site.CDN_PATH}}/public/image/20180530-Personal-NAS-SERVER54.png)

解决方案：

synoboot引导文件版本和群晖DSM的pat版本不一致导致的，包括DS型号（型号一定要一致）和对应的DSM版本（一致或比引导文件版本高）。

###  小结

本次搭建NAS的费用开销有：Gen10 （2479元）+固态硬盘（229元）+SATA机械硬盘（480元*4）+应急电源（265元）+小4P公转SATA线（13.5元），合计4906.5元。基本上相当于央财网上一枚中高端服务器CPU芯片的价格。

完整群晖安装包集合下载： 链接：https://pan.baidu.com/s/1u3kpcMxFC62_jEct24qN9w 密码：8lxb

更多的玩法，可以去泡一泡NAS云论坛【6】。

###  参考

【1】NAS网络存储，https://baike.baidu.com/item/NAS%E7%BD%91%E7%BB%9C%E5%AD%98%E5%82%A8/8856025

【2】Gen10官方文档，https://h20195.www2.hpe.com/v2/getDocument.aspx?docname=a00008701enw

【3】HP MicroServer Gen10开箱及简单测试，http://koolshare.cn/forum.php?mod=viewthread&tid=125017&highlight=gen10

【4】群晖6.1-15101 update3 SSD硬盘引导安装及洗白教程-SSD硬盘自启动小白教程，http://www.nasyun.com/thread-29767-1-2.html

【5】黑群辉DSM 6.1.6-15266 系统安装图文教程，https://www.nas2x.com/threads/dsm-6-1-6-15266-20180402.29/

【6】NAS云论坛，http://www.nasyun.com/