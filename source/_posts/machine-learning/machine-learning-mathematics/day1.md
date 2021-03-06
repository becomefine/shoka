---
title: 机器学习概述
date: 2022-04-13 19:39:17
categories:
 - [机器学习,机器学习算法]
tags: 
 - 机器学习
 - 机器学习算法
 - 数学算法
---

# 全局图: 机器学习路线图

![machine](/assets/machine-learning/zero-book/learning1.png)

## 机器学习的分类

- 按照是否有监督，分为监督学习和无监督学习。
![machine](/assets/machine-learning/zero-book/learning2.png)
![machine](/assets/machine-learning/zero-book/learning3.png)

- 按照预测值是连续还是离散，机器学习分为分类和回归。
![machine](/assets/machine-learning/zero-book/learning4.png)

相对于无监督学习，有监督学习在工业界具有更大的影响力，日常所说的“机器学习”更偏重于“有监督学习”。

## 机器学习过程

![machine](/assets/machine-learning/zero-book/learning5.png)

## 数据准备过程

### 数据采集
![machine](/assets/machine-learning/zero-book/learning6.png)

### 数据清洗

包括数据一致性检查，数据缺失值、错误值或无效值的纠正等。

### 不均衡样本处理

1、如果正负样本数量较多，且正样本远多于负样本，则采用下采样方法来处理。
2、如果正样本远多于负样本，且负样本数量较少，则可以才去上采样方法来处理。

常规的上采样方法有合成少数类过采样技术(Synthetic Minority Oversampling Technique,SMOTE), 即对少数类样本进行分析并根据少数类样本人工合成新样本，将新样本添加到数据集中。

### 数据类型转换

时间型、类别型、文本型、统计型以及组合特征

1、连续数据离散化
- 增加模型稳定性

2、类别数据数值化
- 热独码

### 数据标准化

1、最大值-最小值(max-min)标准化: 最大值-最小值标准化也称为离差标准化，主要是将原始指标缩放到0~1。使用(x-min)(max-min)来代替原特征x。
2、z-score标准化: 首先找到样本特征的均值(mean)和标准差(std)，然后将每个样本特征x变换为(x-mean)/std，从而将数据转换为均值0、标准差为1的正太分布。常用sklearn中的StandardScaler来进行z-score标准化操作

## 特征工程

1、特征构建-通过人工方式从原始数据产生具有物理意义的特征
2、特征提取-通过特征转换方式产生具有统计意义的特征
3、寻找有效特征，剔除不想关或者冗余的特征

特征选择的方法：
1、过滤法
评估某个特征与预测结果之间的相关度，对相关度进行排序，保留特征靠前的特征维度。实践中经常使用pearson相关系数、距离相关度等指标来进行相关度度量。但只考虑了单个特征维度，忽略了特征之间的关联关系，可能存在误删的风险，工业界实践中谨慎使用。
2、包装法
最常用的包装法是递归消除特征法(Recursive Feature Elimination, RFE)。递归消除特征法是将特征选择看作特征子集搜索过程，首先使用全量特征进行算法模型构建，得到基础模型：然后根据线性模型系数，删除部分若特征后观察模型预测能力的变化情况，当模型预测能力大幅下降时停止删除弱特征。
3、嵌入法
主要利用正则化方法做特征选择，使用正则化方法来对特征进行处理，从而特出弱特征。

## 选择算法

### Boosting

从初始训练集中训练一个基学习器，基学习器对不同的样本数据有着不同的预测结果，有些样本基学习器能够很好的预测，有些则不能；对于预测错误的样本，增加其权重后，再次训练下一个基学习器；如此反复进行，知道基学习器数目达到事先指定的数值T，然后将T个基学习器进行加权结合。Boosting算法实际上是算法族，表示一系列将基学习器提升为强学习器的算法，典型的Boosting算法有AdaBoost。AdaBoost算法也完成地体现了上述算法思想。
1、首先在训练集中用初始权重训练一个基学习器1，如果基学习器1对样本m1和m2的分类效果较好，但是对m3的分类效果较差，那么接下来就增加m3的权重，重新训练一个基学习器2.
2、不断重复上述过程，直到基学习器数目达到事先指定的数值T，把这些基学习器通过集合策略进行整合，得到最终的强学习器。
![machine](/assets/machine-learning/zero-book/boosting1.png)

### Bagging和随机森林

1、Bagging算法
在融合模型中，如果个体学习器之间高度保持一致，那么通过融合的方法并不能够提升最后的预测结果。前文讲过，我们需要的个体学习器是“好而不同”的。现实中，虽然各个学习器之间没有办法做到“独立”，但我们可以设法通过尽可能增加每个学习器训练集的差异来使得学习器之间产生较大差异，从而避免各个学习器雷同。这个思想的具体做法：第一，从原始样本集中抽取训练集，每次随机抽取n个训练样本(训练集中，有些样本可能被多次抽取，有些样本则可能一次都没有抽取)，抽取T次得到T个训练集：第二，每次使用一个训练集得到一个模型，T个训练集共得到T个模型；第三，对上述T个学习器才去某种策略进行结合。一般来说，Bagging对分类问题通常采用简单投票法，对回归问题通常采用简单平均法。Bagging算法如下:
![machine](/assets/machine-learning/zero-book/bagging1.png)

2、随机森林算法
随机森林算法思想仍然是Bagging算法思想，但是进行了部分改进，所以随机森林也被看成Bagging的拓展变体。随机森林使用了分类与回归树(Classification and Regression Tree, CART)作为弱学习器，并对决策树的建立做了改进，通过随机选择节点上的一部分样本特征进一步增强了模型的泛化能力。具体来说，随机森林的“随机”主要体现在两方面：数据的随机选择、待选特征的随机选择。

- 数据的随机选择。随机森林从原始数据集中采取有效放回抽样的方式来构造子数据集，并且子数据集的数据量和原始数据集是相同的。
![machine](/assets/machine-learning/zero-book/forest1.png)

- 待选特征的随机选择。随机森林不仅可以实现数据的随机选择，还可以实现待选特征的随机选择。随机森林中子决策树特征的选择步骤是，首先从所有待选特征中随机选取一定的特征，再在这些随机选取的特征中选取最优特征。这样能够使随机森林中的决策树互不相同，通过提升系统多样性从而提升分类性能。


## 调参

1、例如线性回归模型算法: f(x)=w1x1+w2x2+···+wdxd+b。这些参数(w和b)是在模型训练的过程中，计算机根据训练集数据自动得到的。
2、超参数是在模型训练前手动设定。超参数设定的目的是更快、更好地得到算法模型的参数。
3、如果以线性回归算法为例，回归模型一般表达式里面的系数w和b是参数，而正则项的惩罚系数就是超参数。神经网络算法中，节点的权重是参数，而神经网络的层数和每层节点个数就是超参数。


## 欠拟合与过拟合

欠拟合: 模型学习能力较弱，无法学习到样本数据中"一般规律，因此导致模型泛化能力较弱。
解决方法：提高学习器的学习能力，例如在决策树中扩展分支数量或者在神经网络算法中增加训练轮数等。

过拟合：模型学习能力太强，以至于将样本数据中的"个别特点"也当成了"一般规律", 因此导致模型泛化能力较弱。
解决方法：无法避免，只能缓解。例如线性算法中的正则化惩罚项。增加样本量和正则化。

## 算法调参内容

![machine](/assets/machine-learning/zero-book/hyper-parameter.png)

## 回归预测性能度量

1、平均绝对误差
平均绝对误差(Mean Absolute Error, MAE)也称平均绝对离差，是反映预测值与真实值之间离异程度的一种度量，是各预测值偏离真实值的绝对值之和的平均数。平均绝对误差可以避免误差相互抵消的问题，常用于回归预测能力的度量

2、均方误差
均方误差(Mean Square Error, MSE)是反映预测值与真实值之间差异程度的一种度量，是各预测值偏离真实值的距离平方之和的平均数，也即误差平方和的平均数。

## 分类任务性能度量

1、精度与错误率
精度与错误率是分类任务中最常用的两个指标。其中，精度是分类正确的样本占样本总数的比例，错误率是分类错误的样本数占样本总数的比例。

2、查全率与查准率

查全率(召回率)：表示正样品中有多少被真正检测出来
查准率(准确率)

查全率和查准率是一对矛盾体，一般来说如果要求查准率比较高，那么查全率就会比较低；而如果要求查全率比较高，那么查全率就会比较低。
