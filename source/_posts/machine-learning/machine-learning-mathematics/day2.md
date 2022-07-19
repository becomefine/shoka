---
title: 数据降维
math: true
date: 2022-04-25 11:18:19
categories:
 - [机器学习,机器学习算法]
tags: 
 - 机器学习
 - 机器学习算法
 - 数学算法
---

# 数据降维: 深入理解PCA

## PCA

PCA将相关性高的变量转变为较少的新变量，实现用较少的综合指标分别代表存在于各个变量中的各类信息，既减少高维数据的变量维度，又尽量降低原变量数据包含信息的损失程度。PCA保留了高维数据最重要的一部分特征，去除了数据集中的噪声和不重要特征。
PCA在数据挖掘和机器学习实践中的应用:
![machine](/assets/machine-learning/zero-book/PCA1.png)

1、数据可视化。
2、缓解过拟合
3、提高计算性能

PCA过程步骤:
![machine](/assets/machine-learning/zero-book/PCA2.png)

## 寻找降维矩阵P

![machine](/assets/machine-learning/zero-book/PCA3.png)

## PCA降维核心思想

数据降维是因为高维数据中部分维度之间相关性较高造成了数据冗余，但数据降维的过程中又不可避免地造成部分信息的损失。

### 核心思想一: 基变换向量投影

要实现高维向量降维，一个常见方法就是高维向量向低维空间投影

#### 向量投影与向量内积

1、向量内积的代数定义

$\boldsymbol{a}=\left[\begin{array}{c}a_{1} \\ a_{2} \\ a_{3} \\ \vdots \\ a_{n}\end{array}\right]$  $\boldsymbol{b}=\left[\begin{array}{c}b_{1} \\ b_{2} \\ b_{3} \\ \vdots \\ b_{n}\end{array}\right]$

则这两个向量内积为:
$$
\boldsymbol{a} \cdot \boldsymbol{b}=\left[\begin{array}{c}
a_{1} \\
a_{2} \\
a_{3} \\
\vdots \\
a_{n}
\end{array}\right] \cdot\left[\begin{array}{c}
b_{1} \\
b_{2} \\
b_{3} \\
\vdots \\
b_{n}
\end{array}\right]=a_{1} b_{1}+a_{2} b_{2}+a_{3} b_{3}+\cdots+a_{n} b_{n^{\circ}}
$$

2、向量内积的几何定义

向量内积的几何定义用来表征某个向量$\boldsymbol{a}$在另一个向量$\boldsymbol{b}$方向上的投影长度乘以向量$\boldsymbol{b}$的模长，即$\boldsymbol{a} \cdot \boldsymbol{b}=|\boldsymbol{a}||\boldsymbol{b}| \cos \theta_{c}$。如果向量$\boldsymbol{b}$是单位向量(即模长为1)，那么向量$\boldsymbol{a}$与向量$\boldsymbol{b}$的内积结果就等于向量$\boldsymbol{a}$在向量$\boldsymbol{b}$方向上的投影长度$|a| \cos \theta$

##### 向量投影与矩阵向量乘法

1、二维空间
![machine](/assets/machine-learning/zero-book/PCA4.png)

2、推广到m为空间

m为空间向量$\left[\begin{array}{c}x_{1} \\ x_{2} \\ x_{3} \\ \vdots \\ x_{m}\end{array}\right]$实际上位于以m个标准正交基向量$\left[\begin{array}{c}1 \\ 0 \\ 0 \\ \vdots \\ 0\end{array}\right],\left[\begin{array}{c}0 \\ 1 \\ 0 \\ \vdots \\ 0\end{array}\right], \cdots,\left[\begin{array}{c}0 \\ 0 \\ 0 \\ \vdots \\ 1\end{array}\right]$为基底所张成的m维空间中，并且坐标值是向量$\left[\begin{array}{c}x_{1} \\ x_{2} \\ x_{3} \\ \vdots \\ x_{m}\end{array}\right]$分别在标准正交基上的投影值。标准正交基向量作为行向量所构成的$m \times m$矩阵与向量相乘
$$
\left[\begin{array}{ccccc}
1 & 0 & 0 & \cdots & 0 \\
0 & 1 & 0 & \cdots & 0 \\
0 & 0 & 1 & \cdots & 0 \\
\vdots & \vdots & \vdots & & \vdots \\
0 & 0 & 0 & \cdots & 1
\end{array}\right]\left[\begin{array}{c}
x_{1} \\
x_{2} \\
x_{3} \\
\vdots \\
x_{m}
\end{array}\right]=\left[\begin{array}{c}
x_{1} \\
x_{2} \\
x_{3} \\
\vdots \\
x_{m}
\end{array}\right]
$$

### 核心思想二：协方差归零投影

数据降维后的维度之间尽可能相对独立，也就是降维之后的数据维度之间的协方差为0.

1、方差
均值: $\bar{x}=\frac{\sum_{i=1}^{n} x_{i}}{n}$
方差: $S^{2}=\frac{\sum_{i=1}^{n}\left(x_{i}-\bar{x}\right)^{2}}{n-1}$
标准差: $S=\sqrt{\frac{\sum_{i=1}^{n}\left(x_{i}-\bar{x}\right)^{2}}{n-1}}$

2、协方差
协方差度量的是维度和维度之间的关系。
如果两组数据分别是$\mathcal{X}$和$\mathcal{y}$那么这两组数据的协方差为
$$
\operatorname{cov}(x, y)=\frac{\sum_{i=1}^{n}\left(x_{i}-\bar{x}\right)\left(y_{i}-\bar{y}\right)}{n-1}
$$

$\mathcal{X}$越大，同时$\mathcal{y}$也变大，说明两个变量$\mathcal{X}$和$\mathcal{y}$是同向变化的，这时协方差就为正。协方差数值越大，两个变量同向变化的程度也就越高。
$\mathcal{X}$变大，但同时$\mathcal{y}$变小，说明两个变量$\mathcal{X}$和$\mathcal{y}$是反向变化的，这时协方差就为负。协方差数值(绝对值)越大，两个变量反向变化的程度也就越高。
$\mathcal{X}$变化趋势和$\mathcal{y}$变化趋势相互独立的时候，协方差就为0.
协方差是度量各个维度偏离其均值程度的一个指标。协方差为正说明两者是正相关的，协方差为负说明两者是负相关的，协方差为0说明两者的关系就是统计上的“相互独立”。

3、协方差矩阵
假设向量$\boldsymbol{X}=\left(X_{1}, X_{2}, \cdots, X_{n}\right)^{\mathrm{T}}$为n维随机变量，称矩阵
$\boldsymbol{C}=\left(c_{i j}\right)_{n \times n}=\left[\begin{array}{ccccc}C_{11} & C_{12} & C_{13} & \cdots & C_{1 n} \\ C_{21} & C_{22} & C_{23} & \cdots & C_{2 n} \\ C_{31} & C_{32} & C_{33} & \cdots & C_{3 n} \\ \vdots & \vdots & \vdots & & \vdots \\ C_{m 1} & C_{m 2} & C_{m 3} & \cdots & C_{m n}\end{array}\right]$ 为 $n$维随机变量$\boldsymbol{X}$的协方差矩阵，其中$c_{i j}=\operatorname{cov}\left(X_{i}, X_{j}\right),(i, j=1,2, \cdots, n)$为 $X$的分量$X_{i}$和$X_{j}$的协方差。

### 核心思想三: 最大方差投影

原始数据矩阵降维处理之后的新矩阵的维度的方差尽可能大，也就是降维之后矩阵的协方差矩阵的对角线元素尽可能大。

### PCA降维关键：协方差矩阵对角化

PCA降维本质上是通过对矩阵A进行线性变换来实现的，但是这种降维并不是随意的，而是要求降维之后的矩阵$\boldsymbol{Y}$能够最大程度地保持原有矩阵的性质，也就是原始数据的失真程度尽可能小。
降维之后新矩阵$\boldsymbol{Y}$的协方差矩阵$\boldsymbol{C}_{y}$的非对角线元素尽可能为0，而主对角线元素尽可能大。所以降维的实质就是要求降维之后的新矩阵$\boldsymbol{Y}$的协方差矩阵$\boldsymbol{C}_{y}$是对角矩阵。

### PCA降维步骤

1、计算原矩阵$\boldsymbol{A}$的协方差矩阵$\boldsymbol{C}$。
2、计算协方差矩阵$\boldsymbol{C}$的单位正交的特征向量与对应的特征值。
3、根据降维要求，确定$\boldsymbol{k}$值大小。将$\boldsymbol{C}$的特征值从大到小排列，选取$\boldsymbol{k}$个特征值所对应的特征向量。
4、将这些特征向量作为行向量，求解出降维矩阵$\boldsymbol{P}$
5、将降维矩阵$\boldsymbol{P}$乘以原矩阵$\boldsymbol{A}$即可降维，得到$\boldsymbol{Y}=\boldsymbol{P} \boldsymbol{A}$


