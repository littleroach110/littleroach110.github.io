---
layout: post
title: 动态规划之完全背包问题
category: 技术
tags: 动态规划
keywords:
description:
---

### 1. 完全背包问题描述

完全背包问题描述为：有N中物品和一个容量为V的背包，每种物品都可以有无限件可用。第i中物品的费用是c[i]，价值是w[i]，求解将哪些物品装入背包可使这些物品的费用总和不超过背包总量，而且价值总和最大。

### 2. 完全背包问题解决思路

完全背包问题类似于01背包问题，不同的地方在于每种物品有无限件。因此，从每种物品的角度考虑，与其相关的策略并非取或者不取两种，而是有取0件、取1件、取2件...等很多种。如果仍按照每种物品不同的策略写出状态转移方程，如下：

<img src="http://latex.codecogs.com/gif.latex?f[i][v] = max( f[i-1][v-k*c[i]] + k*w[i] | 0 <= k*c[i] <= v )" title="f[i][v] = max( f[i-1][v-k*c[i]] + k*w[i] | 0 <= k*c[i] <= v )" />  

<div>01背包问题是最基本的背包问题，一个简单的方法是将完全背包问题转化成01背包问题。即，考虑到第i种物品最多选V/c[i] 件，则将第i种物品转化为V/c[i]件相同的物品，然后使用01背包的方法，来求解该问题。该问题后续的详细解决方法及代码实现，均可以使用01背包的方法进行实现，求解状态<img src="http://latex.codecogs.com/gif.latex?f[i][v]" title="f[i][v]" /> 时间是<img src="http://latex.codecogs.com/gif.latex?O(v/c[i])" title="O(v/c[i])" /> ，其总的复杂度可认为是<img src="http://latex.codecogs.com/gif.latex?O(V*\sum(V/c[i]))" title="O(V*\sum(V/c[i]))" /> 。

<div>当然，还有更高效的转化方法，特别是用于背包容量V与物品价值c[i]存在较大倍差的时候。这种方法是：将第i种物品拆成费用为<img src="http://latex.codecogs.com/gif.latex?c[i]*2^k" title="c[i]*2^k" /> 、价值为<img src="http://latex.codecogs.com/gif.latex?w[i]*2^k" title="w[i]*2^k" /> 的若干件物品，其中k满足<img src="http://latex.codecogs.com/gif.latex?c[i]*2^k<=V" title="c[i]*2^k<=V" /> 。该方法采用二进制思想，不管最优策略选几件第i种物品，总可以表示成若干个2^k件物品的和，这样将每种物品拆成<img src="http://latex.codecogs.com/gif.latex?O(log V/c[i])" title="O(log V/c[i])" /> 件物品。

>以上方法依然是使用二维数组的方法。

### 3. 完全背包问题优化

<div>完全背包问题有一个很简单有效的优化，是这样的：若两件物品i、j满足<img src="http://latex.codecogs.com/gif.latex?c[i]<=c[j]" title="c[i]<=c[j]" /> 且 <img src="http://latex.codecogs.com/gif.latex?w[i]>=w[j]" title="w[i]>=w[j]"/>，则将物品j去掉，不用考虑。这个优化的正确性显然：任何情况下都可将价值小费用高得j换成物美价廉的i，得到至少不会更差的方案。对于随机生成的数据，这个方法往往会大大减少物品的件数，从而加快速度。然而这个并不能改善最坏情况的复杂度，因为有可能特别设计的数据可以一件物品也去不掉。

### 4. 完全背包问题的一维数组解决方法

先看一段伪代码：

```
for i = 1...N
    for v = 0...V
        f[v] = max( f[v], f[v-cost] + weight )
```

<div>该段代码与01背包问题中的一维数组解决方法很相似，唯一不同的地方在于v变量的循环。01背包问题中，v = V... V-weight，这样做的目的是，避免在进行<img src="http://latex.codecogs.com/gif.latex?" title="" />  f[v] = max( f[v-1], f[v-c[i]] + w[i] ) 的状态推理时，由于先计算数组下标较小f[v]，导致在计算数组下标较大的f[v]时，对物品进行重复计算。而在完全背包问题是，则可以进行多次计算。因此，就需要采用 v=0...V的顺序训话。

### 5. 基于一维数组的代码实现

完全背包问题的代码实现，同样采用Python和C++进行编程，C++代码如下：

```
#include <iostream>
#include <algorithm>

using namespace std;

#define CAPACITY 10
#define NUM 5

int optimal[CAPACITY+1] = {0};
int weight[NUM+1] = {0,4,5,6,2,2};
int value[NUM+1] = {0,6,4,5,3,6};

int main() {
    for(int i = 1; i< NUM+1; i++){
        //for(int j = 1; j< CAPACITY+1; j++){
        for(int j = 1; j< CAPACITY+1; j++){
            if(weight[i] >j)
                optimal[j] = optimal[j-1];
            else
                optimal[j] = max(optimal[j-1],optimal[j-weight[i]]+value[i]);
            //cout <<"optimal["<<j<<"] is "<<optimal[j]<<"\n";
            cout <<optimal[j]<<" ";
        }
        cout << endl;
    }
    cout << endl << "The optimal solution is " << optimal[CAPACITY] << endl;
}
```

运行结果如下：

```
0 0 0 6 6 6 6 12 12 12 
0 0 0 0 4 4 4 4 4 8 
0 0 0 0 0 5 5 5 5 5 
0 3 3 6 6 9 9 12 12 15 
0 6 6 12 12 18 18 24 24 30 

The optimal solution is 30
```

Python代码实现如下：

```
# -*- coding:utf-8 -*-
__author__ = 'leon'
import numpy as np

CAPACITY = 10
NUM = 5

# init array
optimal = [0] * 11
optimal = np.array(optimal)

weight = [0, 4, 5, 6, 2, 2]
value = [0, 6, 4, 5, 3, 6]

for i in range(1,NUM+1):
    for j in range(1,CAPACITY+1):
        if(weight[i] > j):
            optimal[j] = optimal[j-1]
        else:
            optimal[j] = max(optimal[j-1],optimal[j-weight[i]]+value[i])
        print optimal[j],

    print "\n"
print "The optimal solution is ", optimal[10]
```

运行结果如下：

```
0 0 0 6 6 6 6 12 12 12 

0 0 0 0 4 4 4 4 4 8 

0 0 0 0 0 5 5 5 5 5 

0 3 3 6 6 9 9 12 12 15 

0 6 6 12 12 18 18 24 24 30 

The optimal solution is  30
```

<hr>

### 参考
【1】P02: 完全背包问题，http://love-oriented.com/pack/P02.html
【2】背包问题——“完全背包”详解及实现（包含背包具体物品的求解），http://blog.csdn.net/wumuzi520/article/details/7014830