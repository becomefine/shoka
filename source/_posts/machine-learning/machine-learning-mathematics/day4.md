---
title: 聚类
math: true
date: 2022-05-05 15:32:23
categories:
 - [机器学习,机器学习算法]
tags: 
 - 机器学习
 - 机器学习算法
 - 数学算法
---


# 聚类

聚类是机器学习中一种重要的无监督算法，可以将数据点归结为一系列的特定组合。理论上，归为一类的数据点具有相同的特性，而不同类别的数据点则具有各不相同的属性。聚类算法通过将数据点聚集成不同类别，可以揭示数据集中蕴含的一些不为人知的规律，为数据分析师或算法工程师提供更多的分析视角，从而指导生产和生活。

## 聚类算法

聚类分析又称群分析，是研究样品分类或指标分类问题的一种统计分析方法，也是数据挖掘或机器学习中的一个重要算法。

聚类就是将对象分组，使相似的对象归为一类，而不相似的对象归为另一类。聚类和降维之间有着共通性，某种意义上聚类就是降维，聚成K类就意味着将原来的数据降为K维。分类与聚类虽然名称较为接近但两者截然不，分类是有监督学习中的典型问题，而聚类是无监督学习中的典型问题。

![machine](/assets/machine-learning/zero-book/cluster1.png)

## 聚类算法应用场景

聚类可以作为一个单独过程，用来寻找样本数据本省所蕴含的“分布结构”规律，也可以作为有监督学习算法的辅助过程。而聚类作为有监督学习算法的辅助过程，既可以作为前处理过程，也可以作为后处理过程。聚类算法应用场景如下: 
![machine](/assets/machine-learning/zero-book/cluster2.png)

1、聚类作为单独过程。手机用户使用移动网络时会留下用户位置信息，近年来随着地理信息系统(Geographic Information System. GIS)的发展，如何结合用户新与GIS所提供的地理信息来提供商业价值成为一个重要的研究课题。一个最简单的聚类应用就是对移动设备用户信息进行聚类，找到人群聚集点，从而为连锁餐饮机构新店选址提供参考。
2、聚类作为前处理过程。电商平台对新用户进行分类预测(有监督学习)的前提是对样本数据的“用户类型”进行标记，即明确每条用户信息对应哪类用户(例如A类用户、B类用户等)，而对“用户类型”进行标记的前提是对“用户类型”进行定义。聚类算法正好可以对用户群体进行聚类，将每个簇定义为一种用户类型。所以聚类算法可以作为有监督学习的前处理过程。
3、聚类作为后处理过程。运营商设计流量套餐营销方案的时候，可以先通过分类算法识别新用户是否为“流量用户”类型。但是为了实现精准营销，还需要对这些“流量用户”进一步细分其类别，这个时候就可以使用聚类算法实现“流量用户”群体细分。

## 聚类算法过程

聚类算法是无监督学习的典型算法，其中K-means算法又是聚类算法中的经典算法。K-means算法要求预先设定聚类的个数，然后不断更新聚类中心，通过多次迭代最终使得所有数据点到其聚类中心距离的平方和趋于稳定。K-means聚类过程如下：
1、从n个向量对象中任意选择K个对象作为初始聚类中心。
2、根据步骤1中设置的K个聚类中心，分别计算每个对象与这K个聚类中心对象的距离。
3、经过步骤2后，任何一个对象与这K个聚类中心都有一个距离值。这些距离有的远，有的近，将对象与距离它最近的聚类中心归为一类。
4、重新计算每个类簇的聚类中心。
5、重复步骤3和4，直到对象归类变化量极小或者完全停止变化。

:::warning no-icon
1、对象距离如何度量
2、聚类效果如何评价
:::

### 相似度的度量

聚类的实质是按照数据的内在相似性将其划分为多个类别，使得类别内部数据相似度较大而类别间相似度较小。这个“相似度”通过距离来表示，距离越大，相似度越小；距离越小，相似度越大。最常见的距离是“闵可夫斯基距离”：
$$
\operatorname{dist}_{m k}\left(x_{i}, x_{j}\right)=\left(\sum_{u=1}^{n}\left|x_{i u}-x_{j u}\right|^{p}\right)^{\frac{1}{p}}
$$
1、$p=2$时，闵可夫斯基距离就是欧式距离。
$$
\operatorname{dist}\left(x_{i}, x_{j}\right)=\sqrt{\left(\sum_{u=1}^{n}\left|x_{i u}-x_{j u}\right|^{2}\right)}
$$

2、$p=1$时，闵可夫斯基距离就是曼哈顿距离
$$
\operatorname{dist}\left(x_{i}, x_{j}\right)=\sum_{u=1}^{n}\left|x_{i u}-x_{j u}\right|
$$

对于一个垂直布局(如正南正北、正东正西方向规则布局)的城镇街道，从一点$x_{i}$到达另一点$x_{j}$的实际路径长度正是在南北方向上行走的距离。因此，曼哈顿距离又被称为“出租车距离”或者“街区距离”

3、$p \rightarrow \infty$时，闵可夫斯基距离就是切比雪夫距离。
$$
\operatorname{dist}\left(x_{i}, x_{j}\right)=\left(\sum_{u=1}^{n}\left|x_{i u}-x_{j u}\right|^{p}\right)^{\frac{1}{p}}, p \rightarrow \infty
$$

假设$u=k$ 时 $\left|x_{i k}-x_{j k}\right|$是所有$\left|x_{i u}-x_{j u}\right|$中最大的，那么变形可得
$$
\begin{aligned}
\operatorname{dist}\left(x_{i}, x_{j}\right) &=\sqrt[p]{\left|x_{i k}-x_{j k}\right| p\left(\frac{\left|x_{i 1}-x_{j 1}\right|^{p}}{\left|x_{i k}-x_{j k}\right|^{p}}+\frac{\left|x_{i 2}-x_{j 2}\right|^{p}}{\left|x_{i k}-x_{j k}\right|^{p}}+\cdots+\frac{\left|x_{i k}-x_{j k}\right|^{p}}{\left|x_{i k}-x_{j k}\right|^{p}}+\cdots \frac{\left|x_{i n}-x_{j n}\right|^{p}}{\left|x_{i k}-x_{j k}\right|^{p}}\right)} \\
&=\left|x_{i k}-x_{j k}\right| \sqrt[p]{\left(\frac{\left|x_{i 1}-x_{j 1}\right|}{\left|x_{i k}-x_{j k}\right|}\right)^{p}+\left(\frac{\left|x_{i 2}-x_{j 2}\right|}{\left|x_{i k}-x_{j k}\right|}\right)^{p}+\cdots+1+\cdots+\left(\frac{\left|x_{i n}-x_{j n}\right|}{\left|x_{i k}-x_{j k}\right|}\right)^{p}}
\end{aligned}
$$
上式中$\frac{\left|x_{i 1}-x_{j 1}\right|}{\left|x_{i k}-x_{j k}\right|}<1$，所以$\left(\frac{\left|x_{i 1}-x_{j 1}\right|}{\left|x_{i k}-x_{j k}\right|}\right)^{p} \rightarrow 0$。其他同理。上式化简为
$$
\begin{aligned}
{dist}\left(x_{i}, x_{j}\right) &=\left|x_{i k}-x_{j k}\right|
\end{aligned}
$$

除了常用的闵可夫斯基距离之外，还有雅卡尔相似系数、余弦相似度、相对熵、黑林格距离等多种距离计算方法。

### 聚类性能的度量

又称为`有效性指标`。
- 数据含有标记信息。可以考虑使用调整兰德系数(Adjusted Rand Index, ARI)指标。与分类问题中的准确率指标类似。
- 数据不含有标记信息。可以考虑用轮廓系数来度量聚类效果。具有兼顾聚类的凝聚度和分离度的优点。轮廓系数越大，聚类效果越好。

### K-means算法

![machine](/assets/machine-learning/zero-book/K-means1.png)

1、随机选取$K$个初始点作为质心。
2、将数据集中的每个点寻找距离最近的质心，将其分配给该质心对应的簇。
3、将每个簇所有点的均值作为新的质心，替换原来的质心。
4、重复上述过程，直到质心不再变化或者变化很小时停止，否则返回到步骤2.

### K-means++算法

![machine](/assets/machine-learning/zero-book/K-means++1.png)

1、随机选取一个初始点作为第一个质心
2、首先计算每个样本点与当前已有质心的最短距离(即与最近一个质心的距离)，用$D\left(x \right)$表示；接着计算每个样本点被选中作为下一个质心的概率，即$\frac{D\left(x\right)^{2}}{\sum D\left(x\right)^{2}}$值越大表示该点被选为质心的概率越大。
3、用轮盘法选出下一个质心。
4、重复步骤2和3，直到选出$K$个质心。
5、选取$K$个初始质心后，使用标准的K-means算法。


