---
layout: post
title: xgboost在MAC中的安装
category: 技术
tags: TIPS
keywords:
description:
---

### 什么是xgboost

参与Kaggle比赛,自然少不了屠龙神器, XGBoost. 如果你还不知道什么是XGBoost的话, 这是Tianqi Chen开发的一个Gradient Boosting包, 和其他的Gradient Boosting包不同之处在于, XGBoost更快,而且有其他一些很好的特性,比如可以直接用在有缺失值的数据. Tianqi Chen和其他人用XGBoost横扫了Kaggle和其他一些机器学习的比赛, 所以现在基本参加Kaggle比赛的人都会使用XGBoost.

XGBoost是支持多线程的, 要用XGBoost参加Kaggle比赛, 这个功能是必不可少的. 注意, XGBoost的多线程需要用到OpenMP, 也就是编译器需要支持OpenMP, 这也是安装过程中最麻烦的.

### xgboost的安装步骤

##### 1. 取消OSX的SIP保护

Mac在OS X 10.11之后的版本中默认启动了一项系统保护程序，叫做 System Integrity Protection，也被唤作 rootless（寓意让 root 弱一点），该程序意在保护电脑不被恶意程序攻击，但是对于我们这群程序员，很多保护是多余的，甚至给我们带来了很多麻烦。

SIP 会锁定几个系统文件目录：

```
/System
/sbin
/usr （/usr/local 除外）
```

在 SIP 的保护下，部分软件、功能、脚本都会失效，我们可以通过如下步骤关闭 SIP：

* 重启电脑，按下```Command + R ```直到听到开机声音，此时电脑会进入恢复模式（Recovery Mode）

* 当 OSX 工具出现在屏幕中时，下拉工具（Utilities）菜单，选择终端（Terminal）

* 键入``` csrutil disable```，回车

* 电脑重启后，SIP 就关闭了

恢复 SIP 的方式同上，只不过终端中键入```csrutil enable```。通过``` csrutil status```可以检测系统当前 SIP 的启动状态：

```

$ csrutil status

System Integrity Protection status: enabled.
```

##### 2. 安装gcc5

```
brew install gcc5 --without-multilib
```

该阶段持续时间较长，近48分钟，中间会出现疑似卡顿的现象，耐心等等就好。

##### 3. 下载xgboost

```
git clone --recursive https://github.com/dmlc/xgboost
```

##### 4. 编译xgboost

在步骤2中的gcc的安装，其实这时gcc只是下载好了，系统并没有使用刚刚下载下来最新的gcc。

```
cd /usr/bin
rm cc gcc c++ g++
ln -s /usr/local/bin/gcc-5 cc
ln -s /usr/local/bin/gcc-5 gcc
ln -s /usr/local/bin/c++-5 c++
ln -s /usr/local/bin/g++-5 g++
```

对xgboost中的make/config.mk进行修改，去掉下面两行的注释：

```
export CC = gcc
export CXX = g++
```

进行正常编译，该过程需要等待会儿：

```
cp make/config.mk ./config.mk
make -j4
```

##### 5. 安装xgboost python package

该过程也需要持续一阵子

```
cd python-package; 
sudo python setup.py install
```

安装完之后，就可以使用xgboost了

### 参考

【1】MAC OSX下安装XGBOOST，https://zhuanlan.zhihu.com/p/23996104

【2】如何在Mac OSX上安装xgboost，http://www.cnblogs.com/chenhuan001/p/5595380.html

【3】Installation Guide，http://xgboost.readthedocs.io/en/latest/build.html

【4】Unix/Linux 系统中的 Operation Not Permitted 问题，http://www.barretlee.com/blog/2016/04/06/operation-not-permitted-problem-in-linux-or-unix-system/