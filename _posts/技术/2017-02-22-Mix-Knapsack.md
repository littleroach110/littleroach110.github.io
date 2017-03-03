---
layout: post
title: 动态规划之混合三种背包问题
category: 技术
tags: 动态规划
keywords:
description:
---

### 1. 混合三种背包问题描述

混合三种背包问题是将01背包问题、完全背包问题和多重背包问题混合起来考虑，有的物品只可以取一次（01背包），有的物品可以取无限次（完全背包），有的物品可以取的次数有一个上限（多重背包）。

### 2. 混合三种背包问题解决思路

在混合三种背包问题的解决思路中，则是添加了一个判断过程，其伪代码如下：

```
for i = 1...N
    if 第i件物品属于01背包
        使用01背包问题解决方法
    else if 第i件物品属于完全背包
        使用完全背包问题解决方法
    else if 第i件物品属于多重背包
        使用多重背包问题解决方法
```

参考[背包九讲](http://love-oriented.com/pack/)中对01背包问题、完全背包问题和多重背包问题的解决方法，使用函数方法，对于处理过程进行封装。

在01背包问题中，定义过程ZeroOnePack，表示处理一件01背包中的物品，两个参数cost、weight分别表明这件物品的费用和价值。伪代码如下：

```
procedure ZeroOnePack(cost,weight)
      for v=V..cost
            f[v]=max{f[v],f[v-cost]+weight}
```

在完全背包问题中，定义过程CompletePack，表示处理完全背包问题中的物品，其伪代码如下：

```
procedure CompletePack(cost,weight)
      for v=cost..V
            f[v]=max{f[v],f[v-c[i]]+w[i]}
```

在多重背包问题中，定义过程MultiplePack，表示处理多重背包问题中的物品。在O(log amount)时间处理一件多重背包中物品过程的伪代码如下，其中amount表示物品的数量：

```
procedure MultiplePack(cost,weight,amount)
      if cost*amount>=V
            CompletePack(cost,weight)
            return
      integer k=1
      while k<amount
            ZeroOnePack(k*cost,k*weight)
            amount=amount-k
            k=k*2
      ZeroOnePack(amount*cost,amount*weight)
```

因此，对混合三种背包问题的更清晰的写法如下：

```
for i=1..N
      if 第i件物品属于01背包
            ZeroOnePack(c[i],w[i])
      else if 第i件物品属于完全背包
            CompletePack(c[i],w[i])
      else if 第i件物品属于多重背包
            MultiplePack(c[i],w[i],n[i])
```

> 根据背包九讲中的介绍，一些复杂的背包问题，都可以通过拆分为01背包、完全背包和多重背包问题，甚至是混合三种背包的问题来解决。因此，对于三种基本背包问题的思想，需要认真理解。

<hr>
### 参考
【1】P04: 混合三种背包问题，http://love-oriented.com/pack/P04.html
