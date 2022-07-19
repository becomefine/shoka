---
title: NumPy数组操作
date: 2022-05-01 10:10:02
categories:
 - [机器学习,python,numpy]
tags: 
 - numpy
 - 机器学习
 - 数学算法
---

# Numpy数组操作

Numpy中包含了一些函数用于处理数组
- [修改数组形状](#修改数组形状)
- [翻转数组](#翻转数组)
- [修改数组维度](#修改数组维度)
- [连接数组](#连接数组)
- [分割数组](#分割数组)
- [数组元素的添加与删除](#数组元素的添加与删除)

## 修改数组形状

|函数|描述|
|:--|:--|
|reshape|不改变数据的条件下修改形状|
|flat|数组元素迭代器|
|flatten|返回一份数组拷贝，对拷贝所做的修改不会影响原始数组|
|ravel|返回展开数组|

### numpy.reshape

numpy.reshape函数可以在不改变数据的条件下修改形状，格式如下：
```python
numpy.reshape(arr,newshape,order = 'C')
```

- arr: 要修改形状的数组
- newshape: 整数或者整数数组，新的形状应当兼容原有形状
- order: 'C'--按行, 'F'--按列, 'A'--原顺序, 'K' -- 元素在内存中的出现顺序。

```python
import numpy as np

a = np.arrange(8)
print ('原始数组:')
print (a)
print ('\n')

b = a.reshape(4,2)
print ('修改后的数组: ')
print (b)
```
:::default no-icon
原始数组：
[0 1 2 3 4 5 6 7]

修改后的数组：
[[0 1]
 [2 3]
 [4 5]
 [6 7]]
 :::

 ### numpy.ndarray.flat

 numpy.ndarray.flat是一个数组元素迭代器。
 ```python
 import numpy as np

 a = np.arange(9).reshape(3,3)
 print ('原数组: ')
 for row in a:
    print (row)

print ('迭代后的数组: ')
for element in a.flat:
    print (element)
```
输出结果: 
:::default no-icon
原始数组：
[0 1 2]
[3 4 5]
[6 7 8]
迭代后的数组：
0
1
2
3
4
5
6
7
8
:::

### numpy.ndarray.flatten

numpy.ndarray.flatten返回一份数组拷贝，对拷贝所做的修改不会影响原始数组。
```python
ndarray.flatten(order = 'C')
```
- order: 'C'--按行，'F'--按列，'A'--原顺序，'K'--元素在内存中的出现顺序。

### numpy.ravel

numpy.ravel()展平的数组元素，顺序通常是"C风格"，返回的是数组视图(view, 有点类似C/C++引用reference的意味)，修改会影响原始数组。
```python
numpy.ravel(a, order = 'C')
```
- order: 'C'--按行 , 'F'--按列，'A'--原顺序，'K'--元素在内存中的出现顺序。

## 翻转数组

|函数|描述|
|:---|:---|
|transpose|对换数组的维度|
|ndarray.T|和self.transpose()相同|
|rollaxis|向后滚动指定的轴|
|swapaxes|对换数组的两个轴|

### numpy.transpose

numpy.transpose函数用于对换数组的维度。
```python
numpy.transpose(arr, axes)
```
- arr: 要操作的数组
- axes: 整数列表，对应维度，通常所有维度都会对换

### numpy.rollaxis

numpy.rollaxis函数向后滚动特定的轴到一个特定位置。
```python
numpy.rollaxis(arr, axis, start)
```
- arr: 数组
- axis: 要向后滚动的轴，其他轴的相对位置不会改变
- start: 默认为零，表示完整的滚动，会滚动到特定位置。

### numpy.swapaxes

numpy.swapaxes函数用于交换数组的两个轴。
```python
numpy.swapaxes(arr, axis1, axis2)
```
- arr: 输入的数组
- axis1: 对应第一个轴的整数
- axis2: 对应第二个轴的整数

```python
import numpy as np

#创建三维数组
a = np.arrange(8).reshape(2,2,2)

print ('原数组: ')
print (a)
print ('\n')
print('调用swapaxes函数后的数组')
print (np.swapaxes(a, 2, 0))
```
:::default no-icon
原数组：
[[[0 1]
  [2 3]]

 [[4 5]
  [6 7]]]


调用 swapaxes 函数后的数组：
[[[0 4]
  [2 6]]

 [[1 5]
  [3 7]]]
:::

## 修改数组维度

|维度|描述|
|:---|:---|
|broadcast|产生模仿广播的对象|
|broadcast_to|将数组广播到新形态|
|expand_dims|扩展数组的形状|
|squeeze|从数组的形状中删除一维条目|

### numpy.broadcast

numpy.broadcast用于模仿广播的对象，它返回一个对象，该对象封装了将一个数组广播到另一个数组的结果。

### numpy.broadcast_to

numpy.broadcast_to函数将数组广播到新形状。在原始数组上返回只读视图。它通常不连续。如果新形状不符合NumPy的广播规则，该函数可能会抛出ValueError。
```python
numpy.broadcast_to(array, shape, subok)
```

### numpy.expand_dims

numpy.expand_dims函数通过在指定位置插入新的轴来扩展数组形状。
```python
numpy.expand_dims(arr, axis)
```
- arr: 输入数组
- axis: 新轴插入的位置

### numpy.squeeze

numpy.squeeze函数从给定数组的形状中删除一维的条目。
```python
numpy.squeeze(arr, axis)
```
arr: 输入数组
axis: 整数或整数元组，用于选择形状中一维条目的子集

## 连接数组

|函数|描述|
|:---|:--|
|concatenate|连接沿现有轴的数组序列|
|stack|沿着新的轴加入一系列数组|
|hstack|水平堆叠序列中的数组(列方向)|
|vstack|竖直堆叠序列中的数组(行方向)|

### numpy.concatenate

numpy.concatenate函数用于沿指定轴连接相同形状的两个或多个数组。
```python
numpy.concatenate((a1, a2, ...), axis)
```
- a1, a2, ...: 相同类型的数组
- axis: 沿着它连接数组的轴，默认为0

### numpy.stack 

numpy.stack函数用于沿新轴连接数组序列
```python
numpy.stack(arrays, axis)
```
- arrays: 相同形状的数组序列
- axis: 返回数组中的轴，输入数组沿着它来堆叠 

### numpy.hstack

numpy.hstack是numpy.stack函数的变体，通过水平堆叠来生成数组

### numpy.vstack

numpy.vstack是numpy.stack函数的变体，通过垂直堆叠来生成数组。

## 分割数组

|分割|数组及操作|
|:---|:--------|
|split|将一个数组分割为多个子数组|
|hsplit|将一个数组水平分割为多个子数组(按列)|
|vsplit|将一个数组垂直分割为多个子数组(按行)|

### numpy.split

numpy.split函数沿特定的轴将数组分割为子数组。
```python
numpy.split(ary, indices_or_sections, axis)
```
- ary: 被分割的数组
- indices_or_sections: 如果是一个整数，就用该数平均切分，如果是一个数组，为沿轴切分的位置(左开右闭)
- axis: 设置沿着哪个方向进行切分，默认为0，横向切分，即水平方向。为1时，纵向切分，即竖直方向。

axis为0时在水平方向分割，axis为1时在垂直方向分割

### numpy.hsplit

numpy.hsplit函数用于水平分割数组，通过指定要返回的相同形状数量来拆分原数组。
6