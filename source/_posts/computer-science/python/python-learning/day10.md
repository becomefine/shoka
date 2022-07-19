---
title: Python列表(List)
date: 2022-03-09 10:33:08
categories:
 - [计算机科学,Python,Python基础教程]
tags: 
 - 编程技术
 - Python
---
# Python 列表(List)

创建一个列表:
```python
list1 = ['physics', 'chemistry', 1997, 2000]
list2 = [1, 2, 3, 4, 5]
list3 = ["a", "b", "c", "d" ]
```
与字符串的索引一样，列表索引从0开始。列表可以进行截取、组合等。

##访问列表中的值
```python 
#!/usr/bin/python
list1 = ['physics', 'chemistry', 1997, 2000]
list2 = [1, 2, 3, 4, 5, 6, 7 ]

print "list1[0]: ", list1[0]
print "list2[1:5] ", list2[1:5]
```
:::default no-icon
list1[0]: physics
list2[1:5]: [2,3,4,5]
:::

## 更新列表
可以对列表的数据项进行修改或更新，可以使用append()方法来添加列表项。

```python 
#!/usr/bin/python
# -*- coding: UTF-8 -*-

list = []
list.append('Google')
list.append('Runoob')
print list
```

## 删除列表元素
可以使用del语句来删除列表的元素

```python
#!/usr/bin/python

list1 = ['physics', 'chemistry', 1997, 2000]

print list1
del list1[2]
print list1
```

## Python 列表脚本操作符
列表对+和*的操作符与字符串相似。+号用于组合列表, *号用于重复列表。
|Python表达式|结果|描述|
|:-----------|:--|:---|
|len([1,2,3])|3|长度|
|[1,2,3] + [4,5,6]|[1,2,3,4,5,6]|组合|
|['Hi!']*4|['Hi!','Hi!','Hi!','Hi!']|重复|
|3 in [1,2,3]|True|元素是否存在于列表中|
|for x in[1,2,3]: print x, |1 2 3|迭代|

## Python列表截取
|Python表达式|描述|
|:-----------|:---|
|L[2]|读取列表中第三个元素|
|L[-2]|读取列表中倒数第二个元素|
|L[1:]|从第二个元素开始截取列表|

## Python列表函数&方法
|函数|描述|
|:---|:---|
|cmp(list1, list2)|比较两个列表的元素|
|len(list)|列表元素个数|
|max(list)|返回列表元素最大值|
|min(list)|返回列表元素最小值|
|list(seq)|将元组转换为列表|
---------------------------------------------
|方法|描述|
|:---|:---|
|list.append(obj)|在列表末尾添加新的对象|
|list.count(obj)|统计某个元素在列表中出现的次数|
|list.extend(seq)|在列表末尾一次性追加另一个序列中的多个值(用新列表扩展原来的列表)|
|list.index(obj)|从列表中找出某个值第一个匹配项的索引位置|
|list.insert(index, obj)|将对象插入列表|
|list.pop([index = -1])|移除列表中的一个元素(默认最后一个元素), 返回该元素的值。|
|list.remove(obj)|移除列表中某个值的第一个匹配项|
|list.reverse()|反向列表中元素|
|list.sort(cmp=None, key=None, reverse=False)|对原列表进行排序|