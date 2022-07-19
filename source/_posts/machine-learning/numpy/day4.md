---
title: NumPy 从切片和索引
date: 2022-04-29 19:33:11
categories:
 - [机器学习,python,numpy]
tags: 
 - numpy
 - 机器学习
 - 数学算法
---

# Numpy 从数值范围创建数组

ndarray对象的内容可以通过索引或切片来访问和修改，与Python中list的切片操作一样。
ndarray数组切割一个新数组
```python
import numpy as np

a = np.array(10)
s = slice(2,7,2)
print (a[s])
```
:::default no-icon
[2 4 6]
:::
以上实例中，我们首先通过`arrange()`函数ndarray对象。然后，分别设置起始，终止和步长的参数为2,7和2。也可以通过冒号分隔切片参数**start:stop:step**来进行切片操作:
```python
import numpy as np

a = np.arrange(10)
b = a[2:7:2]
print(b)
```
输出结果:
:::default no-icon
[2 4 6]
:::
冒号`:`的解释: 如果只放置一个参数，如`[2]`, 将返回与该索引相对应的单个元素。如果`[2:]`, 表示从该索引开始以后的所有项都将被提取。如果使用了两个参数, 如`[2:7]`, 那么则提取两个索引(不包括停止索引)之间的项

切片还可以包括省略号`...`，来使选择元组的长度与数组的维度相同。如果在行位置使用省略号，他将返回包含行中元素的ndarray。

```python
import numpy as np

a = np.array([1,2,3],[3,4,5],[4,5,6])
print (a[...,1])  # 第2列元素
print (a[1,...])  # 第2行元素
print (a[...,1:]) # 第2列及剩下的所有元素
```
结果:
:::default no-icon
[2 4 4]
[3 4 5]
[[2 3]
 [4 5]
 [5 6]]
:::

# NumPy 高级索引

NumPy比一般的Python序列提供更多的索引方式。除了之前看到的用整数和切片的索引外，数组可以由整数数组索引、布尔索引及花式索引。

## 整数数组索引

以下实例获取数组中(0, 0), (1, 1)和(2, 0)位置处的元素。
```python
import numpy as np

x = np.array([[1, 2], [3, 4], [5, 6]])
# 获取(0,0), (1,1)和(2,0)位置处的元素 
y = x[[0,1,2], [0,1,0]]
print(y)
```
结果:
:::default no-icon
[1 4 5]
:::

以下实例获取了4X3数组的四个角的元素。行索引是[0,0]和[3,3], 而列索引是[0,2]和[0,2]。

```python
import numpy as np

x = np.array([[0, 1, 2], [3, 4, 5], [6, 7, 8], [9, 10 ,11]])
print ('我们的数组是: ')
print (x)
print ('\n')
rows = np.array([[0,0],[3,3]])
cols = np.array([[0,2],[0,2]])
y = x[rows, cols]
print ('这个数组的四个角元素是: ')
print (y)
```
结果:
:::default no-icon
我们的数组是：
[[ 0  1  2]
 [ 3  4  5]
 [ 6  7  8]
 [ 9 10 11]]


这个数组的四个角元素是：
[[ 0  2]
 [ 9 11]]
 :::

 返回的结果是包含每个角元素的ndarray对象。
 可以借助切片`:`或`...`索引数组组合。
 ```python
 import numpy as np

 a = np.array([[1,2,3], [4,5,6], [7,8,9]])
 b = a[1:3, 1:3]
 c = a[1:3, [1,2]]
 d = a[..., 1:]
 print (b)
 print (c)
 print (d)
 ```
 输出结果:
 :::default no-icon
 [[5 6]
 [8 9]]
[[5 6]
 [8 9]]
[[2 3]
 [5 6]
 [8 9]]
 :::
## 布尔索引

可以通过一个布尔数组来索引目标数组。
布尔索引通过布尔运算(如: 比较运算符)来获取符合指定条件的元素的数组。
以下实例获取大于5的元素:
```python
import numpy as np

x = np.array([[0,1,2],[3,4,5],[6,7,8],[9,10,11]])
print(x)
print('\n')
print(x[x > 5])
```
:::default no-icon
[[ 0  1  2]
 [ 3  4  5]
 [ 6  7  8]
 [ 9 10 11]]

[ 6  7  8  9 10 11]
:::
```python
import numpy as np

a = np.array([np.nan, 1,2, np.nan, 3,4,5])
print (a[~np.isnan(a)])
```
:::default no-icon
输出结果:
[ 1.   2.   3.   4.   5.]
:::

```python
import numpy as np

a = np.array([1, 2+6j, 5, 3.5+5j])
print (a[np.iscomplex(a)])
```
:::default no-icon
[2.0+6.j 3.5+5.j]
:::

## 花式索引

花式索引指的是利用整数数组进行索引。
花式索引根据索引数据的值作为目标数组的某个轴的下标来取值。对于使用一维整型数组作为索引，如果目标是一维数组，那么索引的结果就是对应下标的行，如果目标是二维数组，那么就是对应位置的元素。
花式索引跟切片不一样，它总是将数据复制到新数组中。
1、传入顺序索引数组

```python
import numpy as np

x = np.arange(32).reshape(8,4)
print (x[[4,2,1,7]])
```
输出结果:
:::default no-icon
[[16 17 18 19]
 [ 8  9 10 11]
 [ 4  5  6  7]
 [28 29 30 31]]
 :::

 2、传入倒序索引数组

 ```python
 import numpy as np

 x = np.arange(32).reshape((8,4))
 print (x[[-4, -2, -1, -7]])
 ```
 结果:
 :::default no-icon
[[16 17 18 19]
 [24 25 26 27]
 [28 29 30 31]
 [ 4  5  6  7]]
 :::

 3、传入多个索引数组(要使用np.ix_)
 实例:
 ```python
 import numpy as np 
 
x=np.arange(32).reshape((8,4))
print (x[np.ix_([1,5,7,2],[0,3,1,2])])
```
输出结果:
:::default no-icon
[[ 4  7  5  6]
 [20 23 21 22]
 [28 31 29 30]
 [ 8 11  9 10]]
:::


# NumPy 广播(Broadcast)

广播(Broadcast)是numpy对不同形状(shape)的数组进行数值计算的方式，对数组的算术运算通常在相应的的元素上进行。如果两个数组a和b形状相同，即满足**a.shape == b.shape**, 那么a*b的结果就是a与b数组对应位相乘。这要求维数相同，且各维度的长度相同。

实例：
```python
import numpy as np

a = np.array([1,2,3,4])
b = np.array([10,20,30,40])
c = a*b
print (c)
```
:::default no-icon
[ 10 40 90 160]
:::

当运算中的2个数组的形状不同时，numpy将自动触发广播机制。
```python
import numpy as np

a = np.array([[0,0,0],
            [10,10,10],
            [20,20,20],
            [30,30,30]])
b = np.array([1,2,4])
print(a + b)
```
输出结果为:
:::default no-icon
[[1 2 3]
 [11 12 13]
 [21 22 23]
 [31 32 33]]
:::
![machine](/assets/machine-learning/numPy/numPy2.png)

4X3的二维数组长度与长为3的一维数组相加，等效于把数组b在二维上重复4次再运算：

```python
import numpy as np

a = np.array([[0,0,0],
            [10,10,10],
            [20,20,20],
            [30,30,30]])
b = np.array([1,2,3])
bb = np.tile(b,(4,1))
print (a + bb)
```
输出结果:
:::default no-icon
[[ 1  2  3]
 [11 12 13]
 [21 22 23]
 [31 32 33]]
:::

**广播的规则:**
- 让所有输入数组都向其中形状最长的数组看齐，形状中不足的部分都通过在前面加1补齐。
- 输出数组的形状是输入数组形状的各个维度上的最大值。
- 如果输入数组的某个维度和输出数组的对应维度的长度相同或者其长度为1时，这个数组能够用来计算，否则出错。
- 当输入数组的某个维度的长度为1时，沿着此维度运算时都用此维度上的第一组值。

**简单理解:** 对两个数组，分别比较他们的每一个维度(若其中一个数组没有当前维度忽略)，满足:
- 数组拥有相同形状。
- 当前维度的值相等。
- 当前维度的值有一个是1.

# NumPy 迭代数组

NumPy迭代器对象numpy.nditer提供了一种灵活访问一个或者多个数组元素的方式。
迭代器最基本的任务是可以完成对数组元素的访问。
使用arange()函数创建一个2X3数组，并使用nditer对他进行迭代。

```python
import numpy as np

a = np.arange(6).reshape(2,3)
print ('原始数组是: ')
print (a)
print ('\n')
print ('迭代输出元素: ')
for x in np.nditer(a):
    print (x, end = ", ")
print ('\n')
```

输出结果为:
:::default no-icon
原始数组是：
[[0 1 2]
 [3 4 5]]


迭代输出元素：
0, 1, 2, 3, 4, 5, 
:::

以上实例不是使用标准 C 或者 Fortran 顺序，选择的顺序是和数组内存布局一致的，这样做是为了提升访问的效率，默认是行序优先（row-major order，或者说是 C-order）。

这反映了默认情况下只需访问每个元素，而无需考虑其特定顺序。我们可以通过迭代上述数组的转置来看到这一点，并与以 C 顺序访问数组转置的 copy 方式做对比，如下实例
```python
import numpy as np
 
a = np.arange(6).reshape(2,3)
for x in np.nditer(a.T):
    print (x, end=", " )
print ('\n')
 
for x in np.nditer(a.T.copy(order='C')):
    print (x, end=", " )
print ('\n')
```
结果；
:::default no-icon
0, 1, 2, 3, 4, 5, 

0, 3, 1, 4, 2, 5, 
:::

## 控制遍历顺序

- for x in np.nditer(a, order='F'): Fortran order, 即是序列优先；
- for x in np.nditer(a.T, order = 'C'): C order, 即是行序优先。
```python
import numpy as np
 
a = np.arange(0,60,5) 
a = a.reshape(3,4)  
print ('原始数组是：') 
print (a) 
print ('\n') 
print ('原始数组的转置是：') 
b = a.T 
print (b) 
print ('\n') 
print ('以 C 风格顺序排序：') 
c = b.copy(order='C')  
print (c)
for x in np.nditer(c):  
    print (x, end=", " )
print  ('\n') 
print  ('以 F 风格顺序排序：')
c = b.copy(order='F')  
print (c)
for x in np.nditer(c):  
    print (x, end=", " )
```
输出结果:
:::default no-icon
原始数组是：
[[ 0  5 10 15]
 [20 25 30 35]
 [40 45 50 55]]


原始数组的转置是：
[[ 0 20 40]
 [ 5 25 45]
 [10 30 50]
 [15 35 55]]

以 C 风格顺序排序：
[[ 0 20 40]
 [ 5 25 45]
 [10 30 50]
 [15 35 55]]
0, 20, 40, 5, 25, 45, 10, 30, 50, 15, 35, 55, 

以 F 风格顺序排序：
[[ 0 20 40]
 [ 5 25 45]
 [10 30 50]
 [15 35 55]]
0, 5, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55,

:::

# 修改数组中元素的值

nditer对象有另一个可选参数op_flags。默认情况下，nditer将视待迭代遍历的数组为只读对象(read-only), 为了在遍历数组的同时，实现对数组元素值的修改，必须指定read-write或者write-only的模式。

```python
import numpy as np

a = np.arange(0,60,5)
a = a.reshape(3,4)
print ('原始数组是: ')
print (a)
print ('\n')
for x in np.nditer(a, op_flags=['readwrite']):
    x[...] = 2*x
print ('修改后的数组')
print (a)
```
:::default no-icon
原始数组是：
[[ 0  5 10 15]
 [20 25 30 35]
 [40 45 50 55]]


修改后的数组是：
[[  0  10  20  30]
 [ 40  50  60  70]
 [ 80  90 100 110]]
 :::

 ## 使用外部循环

 nditer类的构造器拥有flags参数：
 |参数|描述|
 |:---|:---|
 |c_index|可以跟踪C顺序的索引|
 |f_index|可以跟踪Fortran顺序的索引|
 |multi_index|每次迭代可以跟踪一种索引类型|
 |external_loop|给出的值是具有多个值的一维数组，而不是零维数组|
 
 ```python
 import numpy as np
 a = np.arange(0,60,5)
 a = a.reshape(3,4)
 print('原始数组是: ')
 print(a)
 print('\n')
 print('修改后的数组是: ')
 for x in nd.nditer(a, flags = ['external_loop'], order = 'F'):
    print (x, end = ", ")
```
输出结果为: 
:::default no-icon
原始数组是：
[[ 0  5 10 15]
 [20 25 30 35]
 [40 45 50 55]]


修改后的数组是：
[ 0 20 40], [ 5 25 45], [10 30 50], [15 35 55],
:::


## 广播迭代

如果两个数是可广播的，nditer的组合对象能够同时迭代他们。假设数组a的维度是3X4，数组b的维度为1X4，则使用以下迭代器(数组b被广播到a的大小)。

```python
import numpy as np

a = np.arange(0,60,5)
a = a.reshape(3,4)
print ('第一个数组为: ')
print (a)
print ('\n')
b = np.array([1, 2, 3, 4], dtype = int)
print (b)
print ('\n')
print ('修改后的数组为: ')
for x,y in np.nditer([a,b]):
    print ("%d:%d" % (x,y),end = ", ")

输出结果：
:::default no-icon
第一个数组为：
[[ 0  5 10 15]
 [20 25 30 35]
 [40 45 50 55]]


第二个数组为：
[1 2 3 4]


修改后的数组为：
0:1, 5:2, 10:3, 15:4, 20:1, 25:2, 30:3, 35:4, 40:1, 45:2, 50:3, 55:4,
:::
