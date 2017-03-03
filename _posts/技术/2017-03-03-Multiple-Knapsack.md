---
layout: post
title: 动态规划之多重背包问题
category: 技术
tags: 动态规划
keywords:
description:
---

### 1. 多重背包问题描述

有N种物品和一个容量为V的背包。第i种物品最多有n[i]件可用，每件费用是c[i]，价值是w[i]。求解将哪些物品装入背包可使这些物品的费用总和不超过背包容量，且价值总和最大。

### 2. 多重背包问题解决思路
多重背包问题与完全背包问题相似，只是对于第i种物品有n[i]+1种策略，即取0件、取1件... 取n[i]件。其状态转移方程为：

<img src="http://latex.codecogs.com/gif.latex?f[i][v] = mac( f[i-1][v-k*c[i]] + k*w[i] | 0 \leq k \leq n[i] )" title="f[i][v] = mac( f[i-1][v-k*c[i]] + k*w[i] | 0 \leq k \leq n[i] )" /> 

<div>复杂度是<img src="http://latex.codecogs.com/gif.latex? O(V*\sumn[i] )" title=" O(V*\sumn[i]) " /> 。

对于多重背包问题的基本解决思路是转化为01背包问题：把第i种物品换成n[i]件01背包中的物品，则得到物品数为<img src="http://latex.codecogs.com/gif.latex? \sumn[i] )" title=" \sumn[i] " /> 的01背包问题，复杂度仍然是<img src="http://latex.codecogs.com/gif.latex? O(V*\sumn[i] )" title=" O(V*\sumn[i]) " /> 。

01背包问题的求解过程，在上两篇文章（[01背包](http://littleroach110.net/2017/02/14/Dynamic-Programming-01-Knapsack.html)、[完全背包](http://littleroach110.net/2017/03/02/Complete-Knapsack.html)），已经作了介绍，在此就不赘述。

同样，01背包问题的优化方面，考虑基于二进制的思想，以降低转化为01背包问题的复杂度。

<hr>
### 参考
【1】P03: 多重背包问题，http://love-oriented.com/pack/P03.html
