---
layout: post
title: 动态规划之树形依赖的背包问题
category: 技术
tags: 动态规划
keywords:
description:
---

### 1. 树形依赖的背包问题描述

<div>给定n件物品和一个背包，第i件物品的价值是<img src="http://latex.codecogs.com/gif.latex?w_i" title="w_i" />，其体积为<img src="http://latex.codecogs.com/gif.latex?V_i" title="V_i" />，但是依赖于第<img src="http://latex.codecogs.com/gif.latex?X_i" title="X_i" />件物品（必须选取<img src="http://latex.codecogs.com/gif.latex?X_i" title="X_i" />后才能取i，如果无依赖，则必须选取<img src="http://latex.codecogs.com/gif.latex?X_i = 0" title="X_i = 0" />），依赖关系形成森林，背包的容量为C。可以任意选择装入背包中的物品，求装入背包物品的最大总价值。

当然，按照ACM比赛的问题的提法（参考hdu1561），该问题还有另外一种提法：ACboy很喜欢玩一种战略游戏，在一个地图上，有N座城堡，每座城堡都有一定的宝物，在每次游戏中ACboy允许攻克M个城堡并获得里面的宝物。但由于地理位置原因，有些城堡不能直接攻克，要攻克这些城堡必须先攻克其他某一个特定的城堡。你能帮ACboy算出要获得尽量多的宝物应该攻克哪M个城堡吗？

其中，Input为：每个测试实例首先包括2个整数，N,M.(1 <= M <= N <= 200);在接下来的N行里，每行包括2个整数，a,b. 在第 i 行，a 代表要攻克第 i 个城堡必须先攻克第 a 个城堡，如果 a = 0 则代表可以直接攻克第 i 个城堡。b 代表第 i 个城堡的宝物数量, b >= 0。当N = 0, M = 0输入结束。

Output为：对于每个测试实例，输出一个整数，代表ACboy攻克M个城堡所获得的最多宝物的数量。

### 2. 树形依赖的背包问题解决思路

这里要解析的是直接多叉树动态规划，即直接在原有的树上进行动态规划。从递归的角度来考虑：把根节点V看成是一个容量为V的背包，其所有的子树看成是物品。对于枚举某个子树来说，先去掉这颗子树根节点的体积W，递归求出剩下的V-W容量的背包能装下这个子树的子树的物品。那么将会得到一个从0...V-W的一个01背包，把每一个体积及其对应的价值看成是一个物品，因此共有V-W+1个物品，每个物品算上所有去掉的根节点。那么新的这个V-W+1个物品可以看成是一个分组，物品之间是互斥的，只能选择其一。

由此可见，对于每个跟节点来说，它的所有子树就可以看成是一组组的分组物品，各组内部的物品之间是互斥的，只能选择其一。所以树形依赖的背包问题最终本质是在树上进行的分组背包。

>本质上说，树形依赖的背包问题的解决思路，是针对树的DFS（深度优先搜索）过程和针对各节点的01背包求解过程。深度优先搜索的过程可以通过递归或者循环的方式进行求解。

### 3. hdu 1561解题思路

以树来存储各城堡之间的依赖关系；

首先说状态表示，dp[i][j]表示在以i为根节点的子树上攻破几个城堡可达到的最大收益；

为了方便表示，以0号节点为总的根节点；

接下来使用“背包九讲”中“有依赖的背包问题”一节的思路来做；

“有依赖的背包”是指要选某些物品，必须先选某件物品，这与本题恰好符合。

设j1号，j2号...jn号这n件物品依赖i号物品，那么可以将这n+1个物品归为一组，那么这n+1个物品可能有以下的取法：

只取第i号；

取第i号和第j1号；

取第i号和第j1、j2号；

。。。

取第i号和第j1、j2...jn号；

取第i号和第j2号；

取第i号和第j2、j3号；

。。。

取第i号和第jn号；

这种取法效率很低，存在很多多余情况。可以用01背包的方法来进行优化，即对这n+1种物品进行一次01背包，即可以得出所有需要考虑的情况；

不过因为i的第k个子节点jk也可能被jk的子节点所依赖，所以要通过dfs先把jk的工作做好再来处理。

### 4. 代码实现（接hdu1561）

输入样例

```
3 2
0 1
0 2
0 3
7 4
2 2
0 1
0 4
2 1
7 1
7 6
2 2
0 0
```

输出样例

```
5
13
```

C++实例代码：

```
#include <iostream>
#include <cstring>

using namespace std;

const int Max = 201;

struct edge
{
    int v;
    edge* next;
    edge(int _v, edge* _next) : v(_v), next(_next) {}
}* E[Max];

int N, V, F[Max][Max], C[Max];

void Clear()
{
    memset(F, 0, sizeof(F));
    memset(E, 0, sizeof(E));
}

void Dp(int i, int V)
{
    for (edge* j = E[i]; j; j = j -> next)
    {
        int v = j -> v;
        Dp(v, V - 1);
        for (int k = V; k >= 0; k --)
            for (int l = 0; l < min(V, k); l ++)
                F[i][k] = max(F[i][k], F[i][k - (l + 1)] + F[v][l] + C[v]);
    }
}

inline void edgeAdd(int x, int y)
{
    E[x] = new edge(y, E[x]);
}

void Init()
{
    for (int i = 1, x; i <= N; i ++)
    {
        cin >> x >> C[i];
        edgeAdd(x, i);
    }
}

int main()
{
    cout << "Start Program: \n";
    cin >> N >> V;
    while (N || V)
    {
        Init();
        Dp(0, V);
        cout << F[0][V] << endl;
        Clear();
        cin >> N >> V;
    }
    return 0;
}
```

C++示例代码二(没有做更多的封装，更容易理解整个过程)：

```
#include<iostream>
#include<cstdio>
#include<vector>
using namespace std;
vector<int>list[224];
int dp[224][224];
int value[224];
void DFS( int n ,int M )//M代表攻击该节点时还剩多少次 
{
    int len = list[n].size();
    dp[n][1] = value[n];// 攻击该节点能够得到的财富值
    for( int i = 0 ; i < len ; i++ )//继续攻击该节点下的子节点 
    {
        if( M > 1 )
        DFS( list[n][i] ,M -1 );
        for( int j = M ; j >= 1 ; j-- )// 孩子节点攻击的次数
        {
            int v = j + 1;
            for( int k = 1; k < v ; k++ )// 从孩子节点中找攻击J次的最优值 
            {
                if( dp[n][v] < dp[n][v-k] + dp[list[n][i]][k] )
                    dp[n][v] = dp[n][v-k] + dp[list[n][i]][k];
            }    
        }    
    }
}
int main( )
{
    int  n ,m ,start,end;
    while( scanf( "%d%d",&n ,&m ),n||m )
    {
        memset( dp , 0 , sizeof( dp ) );
        memset( value , 0 , sizeof( value ) );
        for( int i = 0 ; i <= n ; i++ )
        {
             list[i].clear( );
        }
        for( int i = 1 ; i <= n ; i++ )
        {
            scanf( "%d%d",&start , &value[i] );
            list[start].push_back( i );
        }
       DFS( 0 , m+1 );
       printf( "%d\n",dp[0][m+1] );
    }
    return 0;    
}
```

<hr>
### 参考
【1】树形DP-分组背包，http://www.cnblogs.com/GXZC/archive/2013/01/08/2851761.html
【2】树形依赖背包，http://www.cnblogs.com/GXZC/archive/2013/01/13/2858649.html
【3】XTSC 97 选课----树形依赖背包，http://cncc.bingj.com/cache.aspx?q=%E6%A0%91%E5%BD%A2%E4%BE%9D%E8%B5%96%E8%83%8C%E5%8C%85&d=5042632497045505&mkt=zh-CN&setlang=zh-CN&w=WPP7VyHhTr31H9WN1wiaJE1HR7ZbGWAt
【4】hdu-1561，http://acm.hdu.edu.cn/showproblem.php?pid=1561
【5】hdu-1561 The more, The better（树形dp入门，有依赖的背包），http://www.2cto.com/kf/201405/299531.html
