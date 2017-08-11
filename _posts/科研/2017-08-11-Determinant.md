---
layout: post
title: ﻿线性代数之行列式
category: 科研
tags: 线性代数
keywords:
description:
---

﻿> 写在前面：在计算机数学中，线性代数、离散数学、概率论是重要的基础学科。对于深入学习机器学习、人工智能的相关知识，有着重要的作用。因此，将从最基础的内容，开始回顾、整理、归纳和学习。

> 基于 《工程数学-线性代数 第五版》同济大学版 整理和归纳

### 1.什么是行列式

行列式（可能）主要用于求解多元线性方程组，在计算机科学中，即求解多元线性方程组相关的计算问题，讨论使用行列式的方法，根据找到的行列式的“规则”，使用程序去求解问题的解。（承认这一块，目前理解的不够透彻，后续理解将持续更新）

### 2. 行列式的基础计算

##### 2.1 二阶、三阶行列式计算

二阶行列式的计算，通过主对角线乘积减去副对角线乘积。

如图：

![Determinant 01]({{site.CDN_PATH}}/public/image/20170811-Determinant01.png)

其中实线表示主对角线，虚线表示次对角线。

<div>得到的行列式的值为<img src = "http://latex.codecogs.com/gif.latex?a_{11}a_{22}-a_{12}a_{21}" title="a_{11}a_{22}-a_{12}a_{21}" />。</div>

而针对三阶行列式，同样可以使用对角线法则：图中三条实线看作是平行于主对角线的连线，三条虚线看作是平行于副对角线的连线，实线上三元素乘积冠正号，虚线上三元素的乘积冠负号。

![Determinant 02]({{site.CDN_PATH}}/public/image/20170811-Determinant02.png)

记：

![Determinant 03]({{site.CDN_PATH}}/public/image/20170811-Determinant03.png)

##### 2.2 多阶行列式计算

n阶行列式，记作：

![Determinant 04]({{site.CDN_PATH}}/public/image/20170811-Determinant04.png)

<div>简记作<img src = "http://latex.codecogs.com/gif.latex?det(a_{ij})" title="det(a_{ij})" />，其中数<img src = "http://latex.codecogs.com/gif.latex?a_{ij}" title="a_{ij}" />为行列式D的(i,j)元。</div>

考虑到在第三节中的n阶行列式的性质2和性质3，可以通过对行列式进行归并运算，得到三角形行列式，求出主对角线上的行列式的元素的乘积，即可运算出行列式的值，如图所示：

![Determinant 05]({{site.CDN_PATH}}/public/image/20170811-Determinant05.png)

### 3. 行列式的性质和定理

##### 3.1 行列式性质

> 以下行列式的性质，在书中均有完整的证明方法，在实际的使用过程中，牢记并能灵活应用即可。

性质1：行列式与它的转置行列式相等

性质2：互换行列式的两行（列），行列式变号

性质2推论：如果行列式有两行（列）完全相同，则此行列式等于零

性质3：行列式的某一行（列）中所有的元素都乘以同一数k，等于用数k乘以此行列式

性质4：行列式中如果有两行（列）元素成比例，则此行列式等于零

性质5：若此行列式的某一列（行）的元素都是两数之和，则D等于两个行列式之和

性质6：把行列式的某一列（行）的各元素乘以同一数，然后加到另一列（行）对应的元素上去，行列式不变

##### 3.2 行列式定理

> 该部分，需要先介绍余子式和代数余子式的概念

<div>在n阶行列式中，把(i,j)元<img src = "http://latex.codecogs.com/gif.latex?a_{ij}" title="a_{ij}" />所在的第i行和第j列划去后，留下来的n-1阶行列式叫做(i,j)元<img src = "http://latex.codecogs.com/gif.latex?a_{ij}" title="a_{ij}" /> 的余子式，记作<img src = "http://latex.codecogs.com/gif.latex?M_{ij}" title="M_{ij}" />；记：</div>

<img src = "http://latex.codecogs.com/gif.latex?A_{ij}=(-1)^{i+j} M_{ij}" title="A_{ij}=(-1)^{i+j} M_{ij}" />，

<div> <img src = "http://latex.codecogs.com/gif.latex?A_{ij}" title="A_{ij}" /> 叫做(i,j)元<img src = "http://latex.codecogs.com/gif.latex?a_{ij}" title="a_{ij}" />的代数余子式。</div>

定理1：行列式等于它的任一行（列）的各元素与其对应的代数余子式乘积之和

定理1推论：行列式某一行（列）的元素与另一行（列）的对应元素的代数余子式乘积之和等于零

### 4. 克拉默法则

克拉默法则：''' 如果线性方程组的系数行列式不等于零，那么方程有唯一解。'''

克拉默法则主要用于多元线性方程的求解及判定问题，其描述如下：

![Determinant 06]({{site.CDN_PATH}}/public/image/20170811-Determinant06.png)

### 5. 其他内容

##### 5.1 逆序数

##### 5.2 对换


### 参考

【1】《工程数学-线性代数 第五版》同济大学版 ，第一章 行列式