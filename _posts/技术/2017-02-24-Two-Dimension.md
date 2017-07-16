---
layout: post
title: 动态规划之二维费用的背包问题
category: 技术
tags: 动态规划
keywords:
description:
---

### 1. 二维费用的背包问题描述

二维费用的背包问题指的是，对于每件物品，具有两种不同的费用；选择该件物品时，必须同时付出两种代价；对于每种代价，都有一个可付出的最大值（背包容量）。问如何选择物品可以得到最大的价值。

假设这两种代价分别是代价1和代价2，第i件物品所需的两种代价分别是a[i]和b[i]。两种代价可付出的最大值（两种背包容量）分别为V和U。物品的价值为w[i]。

### 2. 二维费用的背包问题解决思路

二维费用较原来一维费用的情况增加了一维，但是不影响对状态转移方程的分析。考虑到增加了一维，则设f[i][v][u]表示前i件物品所付出两种代价分别为v和u时可获得的最大价值。状态转移方程如下：

<img src="http://latex.codecogs.com/gif.latex?f[i][v][u]=max{f[i-1][v][u],f[i-1][v-a[i]][u-b[i]]+w[i]})" title="f[i][v][u]=max{f[i-1][v][u],f[i-1][v-a[i]][u-b[i]]+w[i]}" /> 

根据对[01背包问题](http://littleroach110.net/2017/02/14/Dynamic-Programming-01-Knapsack.html)、[完全背包问题](http://littleroach110.net/2017/03/02/Complete-Knapsack.html)和[多重背包问题](http://littleroach110.net/2017/03/03/Multiple-Knapsack.html)的思考，当每种物品只可以取一次时，变量v和u采用逆序的循环；当物品如完全背包问题时可以取无限次时采用顺序循环；当物品如多重背包问题时，就可以选择拆分物品。

二维费用的01背包问题的伪代码如下：

```
for i = 1...N
    for v = V...v[i]
        for u = U...u[i]
            f[v] = max( f[i-1][v][u],f[i-1][v-a[i]][u-b[i]]+w[i] )
```

二维费用的完全背包问题的伪代码如下：

```
for i = 1...N
    for v = 0...V
        for u = 0...U
            f[v] = max( f[i-1][v][u],f[i-1][v-a[i]][u-b[i]]+w[i] )
```

<hr>

### 参考

【1】二维费用的背包问题，http://love-oriented.com/pack/P05.html
