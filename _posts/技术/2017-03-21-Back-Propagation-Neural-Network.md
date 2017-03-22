---
layout: post
title: BP神经网络相关知识点整理
category: 技术
tags: 神经网络
keywords:
description:
---

### 1. 什么是BP神经网络

BP神经网络，Back Propagation Neural Network，（误差）反向传播神经网络，是一种与最优化方法（如梯度下降法）结合使用的，用来训练人工神经网络的常用方法。该方法计算对网络中所有权重计算损失函数的梯度。这个梯度会反馈给最优化方法，用来更新权值以最小化损失函数。

BP神经网络的主要特点是：信号是向前传播的，而误差是反向传播的。

![BP Neural Network 01]({{site.CDN_PATH}}/public/image/20170320-Back-Propagation-Neural-Network01.jpg)

BP神经网络的过程主要分为两个阶段，第一阶段是信号的前向传播，从输入层经过隐含层，最后到达输出层；第二阶段是误差的反向传播，从输出层到隐含层，最后到输入层，依次调节隐含层到输出层的权重和偏置，输入层到隐含层的权重和偏置。

### 2. BP神经网络的流程

BP神经网络需要依据信号的前向传播和误差的后向传播来构建整个网络，具体的流程和步骤如下：

##### 2.1 网络初始化

假设输入层的节点个数为n，隐含层的节点个数为l，输出层的节点个数为m。输入层到隐含层的权重为<img src="http://latex.codecogs.com/gif.latex?w_{ij}" title="w_{ij}" />，隐含层到输出层的权重为<img src="http://latex.codecogs.com/gif.latex?w_{jk}" title="w_{jk}" />，输入层到隐含层的偏置为<img src="http://latex.codecogs.com/gif.latex?a_j" title="a_j" />，隐含层到输出层的偏置为<img src="http://latex.codecogs.com/gif.latex?b_k" title="b_k" />。学习速率为<img src="http://latex.codecogs.com/gif.latex?\eta" title="\eta" />，激励函数为g(x)。其中激励函数g(x)取Sigmoid函数。形式为：

<img src="http://latex.codecogs.com/gif.latex?g(x) = \frac{1}{1+e^{-x}}" title="g(x) = \frac{1}{1+e^{-x}}" />

##### 2.2 隐含层的输出

如上面的三层BP网络所示，隐含层的输出<img src="http://latex.codecogs.com/gif.latex?H_j" title="H_j" />为：

<img src="http://latex.codecogs.com/gif.latex?H_j = g(\sum^n_{i=1} w_{ij} x_i + a_j)" title="H_j = g(\sum^n_{i=1} w_{ij} x_i + a_j)" />

##### 2.3 输出层的输出

<img src="http://latex.codecogs.com/gif.latex?O_k = \sum^l_{j=1} H_jw_{jk} + b_k" title="O_k = \sum^l_{j=1} H_jw_{jk} + b_k" />

##### 2.4 误差的计算

我们取误差公式为：

<img src="http://latex.codecogs.com/gif.latex?E = \frac{1}{2} \sum^m_{k=1}(Y_k-O_k)^2" title="E = \frac{1}{2} \sum^m_{k=1}(Y_k-O_k)^2" />

其中<img src="http://latex.codecogs.com/gif.latex?Y_k" title="Y_k" />为期望输出。我们记<img src="http://latex.codecogs.com/gif.latex?Y_k-O_k = e_k" title="Y_k-O_k = e_k" />，则E可以表示为：

<img src="http://latex.codecogs.com/gif.latex?E = \frac{1}{2} \sum^m_{k=1}e^2_k" title="E = \frac{1}{2} \sum^m_{k=1}e^2_k" />

以上公式中，<img src="http://latex.codecogs.com/gif.latex?i=1...n, j=1...l, k=1...m" title="i=1...n, j=1...l, k=1...m" />。

##### 2.5 权值的更新

权值的更新公式为：

<img src="http://latex.codecogs.com/gif.latex?\left\{\begin{matrix}&space;\omega&space;_{ij}=\omega&space;_{ij}+\eta&space;H_j\left&space;(&space;1-H_j&space;\right&space;)x_i\sum_{k=1}^{m}\omega&space;_{jk}e_k\\&space;\omega&space;_{jk}=\omega&space;_{jk}+\eta&space;H_je_k&space;\end{matrix}\right." title="gongshi" />

上述公式的计算过程如下：

这是误差反向传播的过程，目标是使得误差函数达到最小值，即min E，使用梯度下降法：

* 隐含层到输出层的权重更新：

<img src="http://latex.codecogs.com/gif.latex?\frac{\partial&space;E}{\partial&space;w_{jk}}=\sum_{k=1}^{m}\left&space;(&space;Y_k-O_k&space;\right&space;)\left&space;(&space;-\frac{\partial&space;O_k}{\partial&space;w_{jk}}&space;\right&space;)=\left&space;(&space;Y_k-O_k&space;\right&space;)\left&space;(&space;-H_j&space;\right&space;)=-e_kH_j" title="">

则权重的更新公式为：

<img src="http://latex.codecogs.com/gif.latex?w_{jk}=w_{jk}+\eta&space;H_je_k" title="gongshi">

* 输入层到隐含层的权重更新：

<img src="http://latex.codecogs.com/gif.latex?\frac{\partial&space;E}{\partial&space;w_{ij}}=\frac{\partial&space;E}{\partial&space;H_j}\cdot&space;\frac{\partial&space;H_j}{\partial&space;\omega&space;_{ij}}" title="">

其中：

<img src="http://latex.codecogs.com/gif.latex?\begin{matrix}&space;\frac{\partial&space;E}{\partial&space;H_j}=\left&space;(&space;Y_1-O_1&space;\right&space;)\left&space;(&space;-\frac{\partial&space;O_1}{\partial&space;H_j}&space;\right&space;)+\cdots&space;+\left&space;(&space;Y_m-O_m&space;\right&space;)\left&space;(&space;-\frac{\partial&space;O_m}{\partial&space;H_j}&space;\right&space;)\\&space;=-\left&space;(&space;Y_1-O_1&space;\right&space;)\omega&space;_{j1}-\cdots-\left&space;(&space;Y_m-O_m&space;\right&space;)\omega&space;_{jm}\\&space;=-\sum_{k=1}^{m}\left&space;(&space;Y_k-O_k&space;\right&space;)\omega&space;_{jk}=-\sum_{k=1}^{m}\omega&space;_{jk}e_k&space;\end{matrix}" title="">

<img src="http://latex.codecogs.com/gif.latex?\begin{matrix}&space;\frac{\partial&space;H_j}{\partial&space;\omega&space;_ij}=\frac{\partial&space;g\left&space;(&space;\sum_{i=1}^{n}\omega&space;_{ij}x_i+a_j&space;\right&space;)}{\partial&space;\omega&space;_ij}\\&space;=g\left&space;(&space;\sum_{i=1}^{n}\omega&space;_{ij}x_i+a_j&space;\right&space;)\cdot&space;\left&space;[&space;1-g\left&space;(&space;\sum_{i=1}^{n}\omega&space;_{ij}x_i+a_j&space;\right&space;)&space;\right&space;]\cdot&space;\frac{\partial&space;\left&space;(&space;\sum_{i=1}^{n}\omega&space;_{ij}x_i+a_j&space;\right&space;)}{\partial&space;\omega&space;_ij}\\&space;=H_j\left&space;(&space;1-H_j&space;\right&space;)x_i&space;\end{matrix}" title="">

则权重的更新公式为：

<img src="http://latex.codecogs.com/gif.latex?\omega&space;_{ij}=\omega&space;_{ij}+\eta&space;H_j\left&space;(&space;1-H_j&space;\right&space;)x_i\sum_{k=1}^{m}\omega&space;_{jk}e_k" title="">

##### 2.6 偏置的更新

偏置的更新公式为：

<img src="http://latex.codecogs.com/gif.latex?\left\{\begin{matrix}&space;a_j=a_j+\eta&space;H_j\left&space;(&space;1-H_j&space;\right&space;)\sum_{k=1}^{m}\omega&space;_{jk}e_k\\&space;b_k=b_k+\eta&space;e_k&space;\end{matrix}\right." title="" >

* 隐含层到输出层的偏置更新

<img src="http://latex.codecogs.com/gif.latex?\frac{\partial&space;E}{\partial&space;b_k}=\left&space;(&space;Y_k-O_k&space;\right&space;)\left&space;(&space;-\frac{\partial&space;O_k}{\partial&space;b_k}&space;\right&space;)=-e_k" title="">

则偏置的更新公式为：

<img src="http://latex.codecogs.com/gif.latex?b_k=b_k+\eta&space;e_k" title="b_k=b_k+\eta&space;e_k">

* 输入层到隐含层的偏置更新：

<img src="http://latex.codecogs.com/gif.latex?\frac{\partial&space;E}{\partial&space;a_j}=\frac{\partial&space;E}{\partial&space;H_j}\cdot&space;\frac{\partial&space;H_j}{\partial&space;a_j}" title="">

其中：

<img src="http://latex.codecogs.com/gif.latex?\begin{matrix}&space;\frac{\partial&space;H_j}{\partial&space;a_j}=\frac{\partial&space;g\left&space;(&space;\sum_{i=1}^{n}\omega&space;_{ij}x_i+a_j&space;\right&space;)}{\partial&space;a_j}\\&space;=g\left&space;(&space;\sum_{i=1}^{n}\omega&space;_{ij}x_i+a_j&space;\right&space;)\cdot&space;\left&space;[&space;1-g\left&space;(&space;\sum_{i=1}^{n}\omega&space;_{ij}x_i+a_j&space;\right&space;)&space;\right&space;]\cdot&space;\frac{\partial&space;\left&space;(&space;\sum_{i=1}^{n}\omega&space;_{ij}x_i+a_j&space;\right&space;)}{\partial&space;a_j}\\&space;=H_j\left&space;(&space;1-H_j&space;\right&space;)&space;\end{matrix}" title="">

<img src="http://latex.codecogs.com/gif.latex?\begin{matrix}&space;\frac{\partial&space;E}{\partial&space;H_j}=\left&space;(&space;Y_1-O_1&space;\right&space;)\left&space;(&space;-\frac{\partial&space;O_1}{\partial&space;H_j}&space;\right&space;)+\cdots&space;+\left&space;(&space;Y_m-O_m&space;\right&space;)\left&space;(&space;-\frac{\partial&space;O_m}{\partial&space;H_j}&space;\right&space;)\\&space;=-\left&space;(&space;Y_1-O_1&space;\right&space;)\omega&space;_{j1}-\cdots-\left&space;(&space;Y_m-O_m&space;\right&space;)\omega&space;_{jm}\\&space;=-\sum_{k=1}^{m}\left&space;(&space;Y_k-O_k&space;\right&space;)\omega&space;_{jk}=-\sum_{k=1}^{m}\omega&space;_{jk}e_k&space;\end{matrix}" title="">

则偏置的更新公式为：

<img src="http://latex.codecogs.com/gif.latex?a_k=a_k+\eta&space;H_j\left&space;(&space;1-H_j&space;\right&space;)\sum_{k=1}^{m}\omega&space;_{jk}e_k" title="">

##### 2.7 判断算法迭代是否结束

有很多的方法可以判断算法是否已经收敛，常用的有指定迭代的代数，判断相邻的两次误差之间的差别是否小于指定的值等。

###  参考

【1】反向传播算法，https://zh.wikipedia.org/wiki/%E5%8F%8D%E5%90%91%E4%BC%A0%E6%92%AD%E7%AE%97%E6%B3%95
 
【2】简单易学的机器学习算法——神经网络之BP神经网络，http://blog.csdn.net/google19890102/article/details/32723459

【3】 神经网络浅讲：从神经元到深度学习，http://www.cnblogs.com/subconscious/p/5058741.html

【4】漫谈ANN（2）：BP神经网络， http://hahack.com/reading/ann2/