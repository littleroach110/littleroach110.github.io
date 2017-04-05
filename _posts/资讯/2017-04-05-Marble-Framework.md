---
layout: post
title: CIA代码混淆框架Marble（PPT分析篇）
category: 资讯
tags: 网络安全
keywords:
description:
---

### 1. 背景及软件简介

2017年3月31日，WikiLeak披露了CIA的秘密反取证Marble框架，Marble框架被用于<b>阻挠</b>取证调查人员和反病毒公司对来自CIA的病毒、木马和黑客攻击行为进行溯源。

Marble通过在CIA的恶意代码中隐藏（混淆）文本片段，以规避目视审查。这是CIA专业工具中的数字装备，用于在将它们投放给CIA秘密支持的叛乱分子之前，将美国开发的武器系统中的英文文本进行覆盖。

Marble是CIA反取证方法和CIA核心恶意代码库的一部分，它被设计为支持灵活和易用的模糊处理方法，其中的字符串混淆算法（特别是那些独特的），通常被用于将恶意代码链接到特定的开发人员或开发商店。

Marble源代码还包括一个调试器，用于反转CIA文本混淆。结合披露的混淆技术，一种模式或签名出现了，它可以帮助取证调查人员对以前的黑客攻击和病毒追溯到CIA。Marble在2016年期间在CIA进行使用，在2015年其达到了1.0版本。

源代码显示，Marble不仅有英文测试示例，还有中文、俄文、韩文、阿拉伯文和波斯文。这将是一个取证溯源双重游戏，例如，通过假装恶意代码创建者的口头语言不是英语，而是中文。但之后限制其企图隐藏隐瞒中文的使用，这样会更加强烈的吸引取证调查人员，并得到错误的结论。但是，还有另外的可能性，比如隐藏假的错误信息。

Marble框架仅用于混淆，本身不包括任何的脆弱性或漏洞。

### 2. Marble介绍PPT

##### 2.1 目标和设计

<b>目标</b>

* 一个混淆框架，不需要我们去拷贝和粘贴太多

* 灵活的，提供了很好的覆盖面

* 不提供一个签名，或帮助减少我们的机会

* 简单易用

* 将其集成到编译阶段（利用事前和事后编译事件？）

<b>设计</b>

* 大算法池

* 使用一个事前编译事件去修改所有的源文件

* 混淆的字符串和数据

* 编译项目

* 使用一个事后编译事件去还原源文件（从不让源崩溃）

* 验证在二进制文件中的所有内容是按照预期进行混淆的

##### 2.2 概念和词汇

* 四部分：Mibster（修饰器）、Mender、Validator、Marbles（算法）

* 从算法池中选择：Mibster选择Marbles

* 存储一份干净的/黄金的源代码备份： Mibster

* 在Visual Studio中自动地使用事前和事后编译事件

* 修改源代码、编译和修复：Mibster和Mender

* 确认：Validator

详细过程包括：

1. Mibster修改源代码，生成接收器
2. 编译项目
3. Mender恢复源代码到源表格
4. 在生成的二进制程序中混淆确认的字符／数据

如图所示：

![Marble Framework01]({{site.CDN_PATH}}/public/image/20170405-Marble-Framework01.png)

##### 2.3 它如何工作

<b>（1）Mibster</b>

[1] 从算法池中选择一个算法

* 1） 默认：从算法池中随机选择

* 2） 选择一个单一算法

* 3） 从算法池中移除集合

* 4） 从算法池中移除单一算法

[2] Marble.h文件是修改你的算法池的方法

[3] 现在有了混淆算法

[4] 浏览目录，寻找源文件（*.c，*.h，*.cpp）

[5] 维持一个文件列表，这些文件包含需要混淆的字符串

[6] 创建环境备份**IMPORTANT**，如果存在问题就失败

[7] 修改源文件——使用混淆的代码和不混乱的代码替换字符串／数据

[8] 生成一个接收器，用于识别算法、文件修改和字符串／数据混淆（对于保证文件生成编译有帮助）

其详细过程包括：

1. 使用Marble.h 从Marble算法池中选择算法
2. 扫描源代码，创建黄金备份
3. 修改源文件
4. 生成接收器

如图所示

![Marble Framework02]({{site.CDN_PATH}}/public/image/20170405-Marble-Framework02.png)

<b>（2）编译项目</b>

编译项目的过程如下：

1. 使用事前编译事件，触发Mibster进行修改
2. 观察输出状态（行号和混淆检查）
3. Mibster中的任何失败都会导致编译失败
4. 你可以经常修改

如图所示：

![Marble Framework03]({{site.CDN_PATH}}/public/image/20170405-Marble-Framework03.png)

<b>（3）Mender</b>

详细过程如下：

1. 扫描任何修改过的源文件
2. 恢复源文件到事前编译状态
3. 注意用户的修改

如图所示：

![Marble Framework04]({{site.CDN_PATH}}/public/image/20170405-Marble-Framework04.png)

<b>（4）Validator</b>

详细过程如下：

1. 使用由Mibster生成的接收器
2. 加载所有的预混淆字符串
3. 对编译的二进制文件进行检查
4. 注意用户的结果

如图所示：

![Marble Framework05]({{site.CDN_PATH}}/public/image/20170405-Marble-Framework05.png)

##### 2.4 用于你的项目

* 使用EDG项目向导

* 核心库仓库（Corelib\Marble）

* 作为子模块添加

* 包含一个ReadMe.txt

* MoveFile（Marble.horig, $(SolutionDir)Shard\Marble.h）

* 在你的项目中包含Marble.h和Deobfuscators

* 添加到项目的“Additional Includes”

* 添加事前和事后编译事件

* 在ReadMe和Confluence中有明确的指导（搜索：Marble）

* 安装后，你可能进行的大部分修改（如果有）在Marble.h中

* 对于每一个Marble，有一个头文件填充注释

* 允许你使用指定的任何一个算法或算法池

* 默认：从整个算法池中随机选择一个

头文件填充注释如图所示：

![Marble Framework06]({{site.CDN_PATH}}/public/image/20170405-Marble-Framework06.png)

选择一个特定的算法，如图所示：

![Marble Framework07]({{site.CDN_PATH}}/public/image/20170405-Marble-Framework07.png)

过滤池：只使用C算法，如图所示：

![Marble Framework08]({{site.CDN_PATH}}/public/image/20170405-Marble-Framework08.png)

排除一个特定的算法，如图所示：

![Marble Framework09]({{site.CDN_PATH}}/public/image/20170405-Marble-Framework09.png)

##### 2.5 示例

支持的typedef：CARBLE和WARBLE

![Marble Framework10]({{site.CDN_PATH}}/public/image/20170405-Marble-Framework10.png)

CARBLE

![Marble Framework11]({{site.CDN_PATH}}/public/image/20170405-Marble-Framework11.png)

WARBLE

![Marble Framework12]({{site.CDN_PATH}}/public/image/20170405-Marble-Framework12.png)

##### 2.6 限制

* CARBLE和WHARBLE必须在函数里使用

* 支持字符串常量和数组

* 使用方括号（[]），而不是星号（*）

* 所有的源文件必须是ANSI、UTF-8或Unicode格式

* 在字符串常量中不支持\U，\u或\ooo

* 当规定\x或0x时，对WARBLE有4个特征，对CARBLE有2个特征

* 字符串常量不能在多行

##### 2.7 文档

* 所有的内容都在Confluence中

* 搜索：Marble或Marble框架

* Marbles的当前列表

* 详细的安装包括EDG项目向导和手工安装

* 图表、描述、定义

* 如何添加框架

* 如何报告事件

* 测试治理

* 等等

##### 2.8 调试和检修

[1] 一个算法有问题？

* 1）从算法池中移除

* 2）报告这个问题

[2] 需要对混淆进行适当的调试

* 1）去除Mender的事前编译事件

* 2）运行 Mibster

* 3）调试

* 4）对代码进行修改

* 5）在修复之前不要做任何修改

### Marble介绍PPT及源代码

PPT下载地址：链接: https://pan.baidu.com/s/1kVdW73L 密码: 41td

源代码下载地址：链接: https://pan.baidu.com/s/1dFGIwtb 密码: f47z

### 参考

【1】Marble Framework，https://wikileaks.org/vault7/#Marble Framework