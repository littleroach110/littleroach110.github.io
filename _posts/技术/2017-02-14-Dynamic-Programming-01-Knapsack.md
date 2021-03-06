---
layout: post
title: 动态规划之01背包问题
category: 技术
tags: 动态规划
keywords:
description:
---

### 1. 01背包问题描述
01背包问题是动态规划算法最经典的例子，动态规划算法通常用来求解最优化问题（Optimization problem），这类问题可以有很多可行解，每个解都有一个值，我们希望寻找具有最优值（最大值或最小值）的解。

01背包问题可以描述为：有N件物品和一个容量为v的背包。第i件物品的费用是c[i]，价值是[i]，求解将哪些物品装入背包，可使价值综合最大。

01背包是基础背包问题，特点是：每种物品仅有一件，可以选择放或不放，不能将物品i装入背包多次，也不能只装入部分的物品i。

### 2. 01背包问题的基本思路
使用01背包问题的子问题来定义状态，其状态转移方程为：

<img src="http://latex.codecogs.com/gif.latex?f[i][v]=max(f[i-1][v],f[i-1][v-c[i]]+w[i])" title="f[i][v]=max(f[i-1][v],f[i-1][v-c[i]]+w[i])" />

其中：

f[i][v]表示前i件物品恰放入一个容量为v的背包可以获得的最大价值；

w[i]表示第i件物品的价值；

c[i]表示第i件物品所占的空间/费用。

<div>这个方程是背包问题的核心状态转移方程，“将前i件物品放入容量为v的背包中”这个问题，如果只考虑第i件物品的策略（放或者不放），就可以转化为一个只牵扯前i-1件物品的问题。如果不放第i件物品，那么问题就转化为“前i-1件物品放入容量为v的背包中”，价值为<img src="http://latex.codecogs.com/gif.latex?f[i-1][v]" title="f[i-1][v]" /> ；如果放入第i件物品，那么问题就转化为"前i-1件物品放入剩下容量为<img src="http://latex.codecogs.com/gif.latex?v-c[i]" title="v-c[i]" /> 的背包中"，此时能获得的最大价值就是<img src="http://latex.codecogs.com/gif.latex?f[i-1][v-c[i]]" title="f[i-1][v-c[i]]" />再加上通过放入第i件物品获得的价值w[i]。（或者这样理解，当开始计算第i个目标时，计算其最大化的价值，主要有两种可能性：1. 不放入i之前，最大的价值是多少；2. 背包的空间减去i的重量，在这个重量下，最大的价值，再加上i的价值。)</div>

### 3. 实际案例描述背包问题
问题描述：有编号分别为a,b,c,d,e 的五件物品，它们的重量分别是2,2,6,5,4，它们的价值分别是6,2,5,4,6，现在给你一个承重为10的背包，如何让背包里装入的物品具有最大的价值总和。

先使用以下的表格，对01背包的动态规划算法及过程进行描述。

<table  class="table table-bordered table-striped table-condensed">
   <tr>
     <th>name</th>
     <th>weight</th>
     <th>value</th>
     <th>1</th>
     <th>2</th>
     <th>3</th>
     <th>4</th>
     <th>5</th>
     <th>6</th>
     <th>7</th>
     <th>8</th>
     <th>9</th>
     <th>10</th>
   </tr>
   <tr>
      <td>a</td>
      <td>2</td>
      <td>6</td>
      <td>0</td>
      <td>6</td>
      <td>6</td>
      <td>9</td>
      <td>9</td>
      <td>12</td>
      <td>12</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
   </tr>
   <tr>
      <td>b</td>
      <td>2</td>
      <td>3</td>
      <td>0</td>
      <td>3</td>
      <td>3</td>
      <td>6</td>
      <td>6</td>
      <td>9</td>
      <td>9</td>
      <td>9</td>
      <td>10</td>
      <td>11</td>
   </tr>
   <tr>
      <td>c</td>
      <td>6</td>
      <td>5</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>6</td>
      <td>6</td>
      <td>6</td>
      <td>6</td>
      <td>6</td>
      <td>10</td>
      <td>11</td>
  </tr>
   <tr>
      <td>d</td>
      <td>5</td>
      <td>4</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>6</td>
      <td>6</td>
      <td>6</td>
      <td>6</td>
      <td>6</td>
      <td>10</td>
      <td>10</td>
   </tr>
   <tr>
      <td>e</td>
      <td>4</td>
      <td>6</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>6</td>
      <td>6</td>
      <td>6</td>
      <td>6</td>
      <td>6</td>
      <td>6</td>
      <td>6</td>
   </tr>
</table>

首先，要明确该表是自底向上、由左到右生成的。为了叙述方便，用e2单元格表示e行2列的单元格，这个单元格的意义是用来表示只有物品e时，有一个承重为2的背包，那么这个背包的最大价值是0，因为e物品的重量是4，背包装不了。

对于d2单元格，表示只有e,d时，承重为2的背包，所能装入的最大价值，仍然是0，因为物品e,d都不是这个背包能装的。同理，c2=0，b2=3，a2=6。

对于承重为8的背包，a8=15是怎么得出的呢？

<div>根据01背包的状态转换方程，需要考察两个值：一个是<img src="http://latex.codecogs.com/gif.latex?f[i-1][j]" title="f[i-1][j]" /> ，对于这个例子来说，就是b8的值9；另一个是<img src="http://latex.codecogs.com/gif.latex?f[i-1][v-c[i]]+w[i]" title="f[i-1][v-c[i]]+w[i]" /> 。在这里，<img src="http://latex.codecogs.com/gif.latex?f[i-1][j]" title="f[i-1][j]" /> 表示我有一个承重为8的背包，当只有b,c,d,e四件可选是，这个背包能装入的最大价值；<img src="http://latex.codecogs.com/gif.latex?f[i-1][v-c[i]]" title="f[i-1][v-c[i]]" /> 表示我有一个承重为6的背包（等于当前背包承重减去物品a的重量），当只有物品b,c,d,e四件可选时，这个背包能装入的最大价值。<img src="http://latex.codecogs.com/gif.latex?f[i-1][v-c[i]]" title="f[i-1][v-c[i]]" /> 就是指单元格b6，其值为9，w[i]指的是a物品的价值，即6。</div>

<div>由于<img src="http://latex.codecogs.com/gif.latex?f[i-1][v-c[i]]+w[i] = 9+6=15" title="f[i-1][v-c[i]]+w[i] = 9+6=15" /> 大于<img src="http://latex.codecogs.com/gif.latex?f[i-1][j]=9" title="f[i-1][j]=9" /> ，所以物品a应该放入承重为8的背包。</div>

### 4. 基于二维数组的代码实现
该部分的01背包问题的代码实现，主要依赖二维数组，分别使用Python和C++进行程序编写。

<div> python代码实现过程中，使用numpy库实现对二维数组的定义和赋值操作；另外，在Python和C++的代码实现中，考虑到<img src="http://latex.codecogs.com/gif.latex?optimal[i][j] = optimal[i-1][j]" title="optimal[i][j] = optimal[i-1][j]" /> 中的i-1操作，为避免出现数组下标为负值，在定义weight和value时，对weight[0]和value[0]赋值为0。</div>

其中，Python代码实现如下：

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


### 5. 基于一维数组的代码实现

<div>使用一维数组时，01背包问题的本质状态转移方程并没有改变。在使用一维数组f[0...v]，要达到的效果是：第i次循环结束后，f[v]中所表示的就是二维数组时的<img src="http://latex.codecogs.com/gif.latex?f[i][v]" title="f[i][v]" />，即前i个物体面对容量时的最大价值。在二维数组中，可知<img src="http://latex.codecogs.com/gif.latex?f[i][v]" title="f[i][v]" />由两个状态得来: <img src="http://latex.codecogs.com/gif.latex?f[i-1][v]" title="f[i-1][v]" />和<img src="http://latex.codecogs.com/gif.latex?f[i-1][v-c[i]]+w[i]" title="f[i-1][v-c[i]]+w[i]" />。使用一维数组时，当第i次循环之前时，f[v]实际就是<img src="http://latex.codecogs.com/gif.latex?f[i-1][v]" title="f[i-1][v]" />；而对于<img src="http://latex.codecogs.com/gif.latex?f[i-1][v-c[i]]" title="f[i-1][v-c[i]]" />，当每次循环中，以v=v...0的顺序推f[v]时，就能保证<img src="http://latex.codecogs.com/gif.latex?f[v-c[i]]" title="f[v-c[i]]" />的状态。其状态转移方程为：</div>

<img src="http://latex.codecogs.com/gif.latex?v= V...0; f[v] = max( f[v], f[v-c[i]]+w[i] )" title="v= V...0; f[v] = max( f[v], f[v-c[i]]+w[i] )" />

此状态转移方程对应于二维数组的状态转移方程如下：

<img src="http://latex.codecogs.com/gif.latex?f[i][v]=max(f[i-1][v],f[i-1][v-c[i]]+w[i])" title="f[i][v]=max(f[i-1][v],f[i-1][v-c[i]]+w[i])" />

<div>如上所示，<img src="http://latex.codecogs.com/gif.latex?f[v-c[i]]" title="f[v-c[i]]" /> 相当于二维数组中的<img src="http://latex.codecogs.com/gif.latex?f[i-1][v-c[i]]" title="f[i-1][v-c[i]]" /> 。而如果将v的循环顺序由逆序改为顺序，就不是01背包问题，而是完全背包问题，即一个物品可能会重复装入到背包中。举个例子，假如由物品z的容量为2，价值wz很大，背包容量为5，如果v的循环顺序不是逆序，那么外层循环跑到物体z时，内循环在v=2时，物品z被放入背包，当v=4时，寻求最大价值，物品z放入背包，<img src="http://latex.codecogs.com/gif.latex?f[4] = max( f[4], f[2]+ wz)" title="f[4] = max( f[4], f[2]+ wz)" />，后者比前者大，但此时<img src="http://latex.codecogs.com/gif.latex?f[2]+ wz" title="f[2]+ wz" /> 中的f[2]已经装入了一次物体z，这样一来，该物品z被装入背包2次，不符合要求。当使用逆序循环 v = V...0 时，则先计算数组下标较大的f[v]，以此不会发生一个物品被装入2次的情况。</div>

### 6. 基于一维数组的代码实现

同样，使用Python和C++对一维数组的01背包问题进行代码实现，其中，Python实现代码如下：

```
# -*- coding:utf-8 -*-
import numpy as np

CAPACITY = 10
NUM = 5

# init array
optimal = [0] * 11
optimal = np.array(optimal)

weight = [0, 4, 5, 6, 2, 2]
value = [0, 6, 4, 5, 3, 6]

for i in range(1,NUM+1):
    for j in range(weight[i],CAPACITY+1)[::-1]:
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
6 6 6 6 6 6 6 

10 10 6 6 6 6 

11 6 6 6 6 

9 9 9 9 9 6 3 3 3 

15 15 15 12 9 9 9 6 6 

The optimal solution is  15
```

C++代码实现过程如下：

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
        for(int j = CAPACITY; j>= weight[i]; j--){
            if(weight[i] >j)
                optimal[j] = optimal[j-1];
            else
                optimal[j] = max(optimal[j-1],optimal[j-weight[i]]+value[i]);
            cout <<optimal[j]<<" ";
        }
                cout << endl;
    }
    cout << endl << "The optimal solution is " << optimal[CAPACITY] << endl;
}
```

运行结果如下：

```
6 6 6 6 6 6 6 
10 10 6 6 6 6 
11 6 6 6 6 
9 9 9 9 9 6 3 3 3 
15 15 15 12 9 9 9 6 6 

The optimal solution is 15
```

<hr>
### 参考：

【1】01背包问题，https://www.youtube.com/watch?v=m_CmyldCRI8

【2】P01: 01背包问题，http://love-oriented.com/pack/P01.html

【3】动态规划之01背包问题（最易理解的讲解），http://blog.csdn.net/mu399/article/details/7722810

【4】01背包算法 动态规划（c++实现），http://blog.csdn.net/catkint/article/details/51009680

【5】0-1背包使用一维数组，http://blog.csdn.net/fobdddf/article/details/19479217