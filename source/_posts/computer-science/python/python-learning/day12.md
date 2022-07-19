---
title: Python字典
date: 2022-03-09 11:54:27
categories:
 - [计算机科学,Python,Python基础教程]
tags: 
 - 编程技术
 - Python
---

# Python 字典(Dictionary)

字典是另一种可变容器模型，且可存储任意类型对象。
字典的每个键值`key=>value`对用冒号`:`分割, 每个键值对之间用逗号`,`分割, 整个字典包括在花括号`{}`中。
```python
d = {key1 : value1, key2 : value2}
```
:::info
注意: `dict`作为Python的关键字和内置函数，变量名不建议命名为**dict**
键一般是唯一的，如果重复最后的一个键值对会替换前面的，值不需要唯一。
:::

```python 
\>>> tinydict = {'a': 1, 'b': 2, 'b': '3'}
\>>> tinydict['b']
'3'
\>>> tinydict
{'a': 1, 'b': '3'}
```

值可以取任何数据类型，但键必须是不可变的，如字符串，数字或元组。

## 访问字典里的值

```python
# !/usr/bin/python

tinydict = {'Name': 'Runoob', 'Age': 7, 'Class': 'First'}

print "tinydict['Alice']: ", tinydict['Alice']
```

## 修改字典
向字典添加新内容的方法是增加新的键值对，修改或删除已有键/值对:
```python
# !/usr/bin/python
# -*- coding: UTF-8 -*-

tinydict = {'Name': 'Zara', 'Age': 7, 'Class': 'First'}

tinydict['Age'] = 8   #更新
tinydict['School'] = "RUNOOB" #添加
del tinydict['Name']  #删除键是'Name'的条目
tinydict.clear()      #情况字典所有条目
del tinydict          #删除字典

print "tinydict['Age']: ", tinydict['Age']
print "tinydict['School']: ", tinydict['School']
```

## 字典键的特性
字典值可以没有限制的取任何python对象，既可以是标准的对象，也可以是用户定义的，但键不行。
:::info
1) 不允许同一个键出现两次。创建时如果同一个键被赋值两次，后一个值会被记住。
2) 键必须不可变，所以可以用数字，字符串或元组充当，所以用列表就不行。
:::

## 字典内置函数&方法

内置函数:
|函数|描述|
|:---|:---|
|cmp(dict1, dict2)|比较两个字典元素)|
|len(dict)|计算字典元素个数，即键的总数|
|str(dict)|输出字典可打印的字符串表示|
|type(variable)|返回输入的变量类型, 如果变量是字典就返回字典类型|

内置方法: 
|方法|描述|
|:---|:---|
|dict.clear()|删除字典内所有元素|
|dict.copy()|返回一个字典的浅复制|
|dict.fromkeys(seq[,val])|创建一个新字典，以序列seq中元素做字典的键，val为字典所有键对应的初始值|
|dict.get(key, default=None)|返回指定键的值，如果值不在字典中返回default值|
|dict.has_key(key)|如果键在字典dict里返回true, 否则返回false.|
|dict.items()|以列表返回可遍历的(键、值)元组数组。|
|dict.keys()|以列表返回一个字典所有的键|
|dict.setdefault(key, default=None)|和get()类似，但如果键不存在于字典中，将不会添加键并将值设为default|
|dict.update(dict2)|把字典dict2的键/值对更新到dict里|
|dict.values()|以列表返回字典中的所有值|
|pop(key[,default])|删除字典给定键key所对应的值，返回值为被删除的值。key值必须给出。否则，返回default值。|
|popitem()|返回并删除字典中的最后一对键和值|

