---
layout: post
title: 动态规划之01背包问题（二维数组）
category: 技术
tags: 动态规划
keywords:动态规划，最优化，01背包
description:介绍动态规划中的01背包问题的思考、解决及编程实现的过程
---

### 1. 01背包问题描述
01背包问题是动态规划算法最经典的例子，动态规划算法通常用来求解最优化问题（Optimization problem），这类问题可以有很多可行解，每个解都有一个值，我们希望寻找具有最优值（最大值或最小值）的解。

01背包问题可以描述为：有N件物品和一个容量为v的背包。第$i$件物品的费用是c[i]，价值是[i]，求解将哪些物品装入背包，可使价值综合最大。

01背包是基础背包问题，特点是：每种物品仅有一件，可以选择放或不放，不能将物品i装入背包多次，也不能只装入部分的物品$i$。

### 2. 01背包问题的基本思路
使用01背包问题的子问题来定义状态，其状态转移方程为：

<img src="http://latex.codecogs.com/gif.latex?f[i][v]=max(f[i-1][v],f[i-1][v-c[i]]+w[i])" title="f[i][v]=max(f[i-1][v],f[i-1][v-c[i]]+w[i])" />

其中：
f[i][v]表示前i件物品恰放入一个容量为v的背包可以获得的最大价值；

w[i]表示第i件物品的价值；

c[i]表示第i件物品所占的空间/费用。

这个方程是背包问题的核心状态转移方程，“将前i件物品放入容量为v的背包中”这个问题，如果只考虑第i件物品的策略（放或者不放），就可以转化为一个只牵扯前i-1件物品的问题。如果不放第i件物品，那么问题就转化为“前i-1件物品放入容量为v的背包中”，价值为<img src="http://latex.codecogs.com/gif.latex?f[i-1][v]" title="f[i-1][v]" /> ；如果放入第i件物品，那么问题就转化为"前i-1件物品放入剩下容量为<img src="http://latex.codecogs.com/gif.latex?v-c[i]" title="v-c[i]" /> 的背包中"，此时能获得的最大价值就是<img src="http://latex.codecogs.com/gif.latex?f[i-1][v-c[i]]" title="f[i-1][v-c[i]]" />再加上通过放入第i件物品获得的价值w[i]。（或者这样理解，当开始计算第i个目标时，计算其最大化的价值，主要有两种可能性：[1] 不放入i之前，最大的价值是多少；[2] 背包的空间减去i的重量，在这个重量下，最大的价值，再加上i的价值。)

### 3. 实际案例描述背包问题
问题描述：有编号分别为a,b,c,d,e 的五件物品，它们的重量分别是2,2,6,5,4，它们的价值分别是6,2,5,4,6，现在给你一个承重为10的背包，如何让背包里装入的物品具有最大的价值总和。

先使用以下的表格，对01背包的动态规划算法及过程进行描述。

|name|weight|value|1|2|3|4|5|6|7|8|9|10|
|-------|-------|-------|----|----|----|----|----|----|----|----|----|----|
|a|2|6|0|6|6|9|9|12|12|15|15|15|
|b|2|6|0|3|3|6|6|9|9|9|10|11|
|c|6|5|0|0|0|6|6|6|6|6|10|11|
|d|5|4|0|0|0|6|6|6|6|6|10|10|
|e|4|6|0|0|0|6|6|6|6|6|6|6|

首先，要明确该表是自底向上、由左到右生成的。为了叙述方便，用e2单元格表示e行2列的单元格，这个单元格的意义是用来表示只有物品e时，有一个承重为2的背包，那么这个背包的最大价值是0，因为e物品的重量是4，背包装不了。

对于d2单元格，表示只有e,d时，承重为2的背包，所能装入的最大价值，仍然是0，因为物品e,d都不是这个背包能装的。同理，c2=0，b2=3，a2=6。

对于承重为8的背包，a8=15是怎么得出的呢？

根据01背包的状态转换方程，需要考察两个值：一个是<img src="http://latex.codecogs.com/gif.latex?f[i-1][j]" title="f[i-1][j]" /> ，对于这个例子来说，就是b8的值9；另一个是<img src="http://latex.codecogs.com/gif.latex?f[i-1][v-c[i]]+w[i]" title="f[i-1][v-c[i]]+w[i]" /> 。在这里，<img src="http://latex.codecogs.com/gif.latex?f[i-1][j]" title="f[i-1][j]" /> 表示我有一个承重为8的背包，当只有b,c,d,e四件可选是，这个背包能装入的最大价值；<img src="http://latex.codecogs.com/gif.latex?f[i-1][v-c[i]]" title="f[i-1][v-c[i]]" /> 表示我有一个承重为6的背包（等于当前背包承重减去物品a的重量），当只有物品b,c,d,e四件可选时，这个背包能装入的最大价值。<img src="http://latex.codecogs.com/gif.latex?f[i-1][v-c[i]]" title="f[i-1][v-c[i]]" /> 就是指单元格b6，其值为9，w[i]指的是a物品的价值，即6。

由于<img src="http://latex.codecogs.com/gif.latex?f[i-1][v-c[i]]+w[i] = 9+6=15" title="f[i-1][v-c[i]]+w[i] = 9+6=15" /> 大于<img src="http://latex.codecogs.com/gif.latex?f[i-1][j]=9" title="f[i-1][j]=9" /> ，所以物品a应该放入承重为8的背包。

### 4. 基于二维数组的代码实现
该部分的01背包问题的代码实现，主要依赖二维数组，分别使用Python和C++进行程序编写。

> python代码实现过程中，使用numpy库实现对二维数组的定义和赋值操作；另外，在Python和C++的代码实现中，考虑到<img src="http://latex.codecogs.com/gif.latex?optimal[i][j] = optimal[i-1][j]" title="optimal[i][j] = optimal[i-1][j]" /> 中的i-1操作，为避免出现数组下标为负值，在定义weight和value时，对weight[0]和value[0]赋值为0。

其中，Python代码实现如下
```
# -*- coding:utf-8 -*-
import numpy as np

CAPACITY = 10
NUM = 5

# init array
optimal = [[0] * 11] * 6
optimal = np.array(optimal)

weight = [0, 4, 5, 6, 2, 2]
value = [0, 6, 4, 5, 3, 6]

for i in range(1,NUM+1):
    for j in range(1,CAPACITY+1):
        if(j < weight[i]):
            optimal[i][j] = optimal[i-1][j]
            print optimal[i][j],
        else:
            optimal[i][j] = max(optimal[i-1][j], optimal[i-1][j-weight[i]] + value[i])
            print optimal[i][j],
    print "\n"
print "The optimal solution is ", optimal[5][10]
```
其运行结果为：
```
0 0 0 6 6 6 6 6 6 6 

0 0 0 6 6 6 6 6 10 10 

0 0 0 6 6 6 6 6 10 11 

0 3 3 6 6 9 9 9 10 11 

0 6 6 9 9 12 12 15 15 15 

The optimal solution is  15
```


C++的代码实现如下：
```
#include <iostream>
#include <algorithm>

using namespace std;

#define CAPACITY 10
#define NUM 5

int optimal[NUM+1][CAPACITY+1] = {0};
int weight[NUM+1] = {0,4,5,6,2,2};
int value[NUM+1] = {0,6,4,5,3,6};

int main() {
    for(int i = 1; i< NUM+1; i++){
        for(int j = 1; j < CAPACITY+1; j++){
            if(weight[i] >j){
                optimal[i][j] = optimal[i-1][j];
                cout<< optimal[i][j]<< ' ';
            }
            else{
                optimal[i][j] = max(optimal[i-1][j],optimal[i-1][j-weight[i]] + value[i]);
                cout<< optimal[i][j]<< ' ';
            }
        }
        cout << endl;
    }
    cout << endl << "The optimal solution is " << optimal[NUM][CAPACITY] << endl;
}
```
其运算结果为：
```
0 0 0 6 6 6 6 6 10 10 
0 0 0 6 6 6 6 6 10 11 
0 3 3 6 6 9 9 9 10 11 
0 6 6 9 9 12 12 15 15 15 

the optimal solution is 15
```
<br>
### 参考：
【1】01背包问题，https://www.youtube.com/watch?v=m_CmyldCRI8
【2】P01: 01背包问题，http://love-oriented.com/pack/P01.html
【3】动态规划之01背包问题（最易理解的讲解），http://blog.csdn.net/mu399/article/details/7722810
【4】01背包算法 动态规划（c++实现）http://blog.csdn.net/catkint/article/details/51009680