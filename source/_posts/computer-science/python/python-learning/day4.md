---
title: python变量类型
date: 2022-02-28 22:07:16
categories:
 - [计算机科学,Python,Python基础教程]
tags: 
 - 编程技术
 - Python
---

# Python 变量类型

## 多个变量赋值
```python
a = b = c = 1
#创建一个整形对象，值为1，三个变量被分配到相同的内存空间上
a, b, c = 1, 2, "john"
#两个整形对象1和2分别分配给变量a和b，字符串对象"john"分配给变量c
```
------------------------------

## 标准数据类型
- Numbers(数字)
- String(字符串)
- List(列表)
- Tuple(元组)
- Dictionary(字典)
--------------------------------

## Python数字
数字数据类型用于存储数值。
他们是不可改变的数据类型，这意味着改变数字类型会分配一个新的对象。
当你指定一个值时，Number对象就会被创建：
```python
var1 = 1
var2 = 2
```
可以使用del语句删除一些对象的引用。
del语句的语法是：
```python
del var1[,var2[,var3[....,varN]]]
```

可以通过使用del语句删除单个或多个对象的引用。例如：
```python
del var
del var_a, var_b
```

Python支持四种不同类型的数字类型：
- int(有符号整型)
- long(长整型，也可以代表八进制和十六进制)
- float(浮点型)
- complex(复数)
实例：
一些数值类型的实例：
|int      |long     |float   |complex  |
|:--------|:--------|:-------|:--------|
|10       |5192436L |0.0     |3.14j    |
|-786     |0122L    |-21.9   |45.j     |
|080      |0xDEFABCE|32.3e+18|.876j    |

- 长整型也可以使用小写l，但是还是建议使用大写L，避免与数字1混淆。Python使用L来显示长整型
- Python还支持复数，复数由实数部分和虚数部分构成，可以用a+bj，或者complex(a,b)表示，复数的实部a和虚部b都是浮点型。

[注意：long类型只存在于Python2.X版本中，在2.2以后的版本中，int类型数据溢出后会自动转为long类型。在Python3.X版本中long类型被移除，使用int替代]{.label .warning}


## Python字符串
字符串或串(String)是由数字、字母、下划线组成的一串字符。
一般记为：
```python
s = "a1a2···an" #n>=0
```
他是编程语言中表示文本的数据类型。
python的字串列表有2种取值顺序：
- 从左到右索引默认0开始的，最大范围是字符串长度少1
- 从右到左索引默认-1开始的，最大范围是字符串开头
![python2](/assets/python/python2.png)
如果你要实现从字符串中获取一段子字符串的话，可以使用`[头下标:尾下标]`来截取相应的字符串，其中下标是从0开始算起，可以是正数或负数，下标可以为空表示取到头或尾。
比如：
```python
>>> s = 'abcdef'
>>> s[1:5]
'bcde'
```

当使用以冒号分隔的字符串，python返回一个新的对象，结果包含了以这对偏移标识的连续的内容，左边的开始是包含了下边界。
上面的结果包含了`s[1]`的值b，而取到的最大范围不包括**尾下标**，就是`s[5]`的值f。
![python3](/assets/python/python3.png)
加号(+)是字符串连接运算符，星号(*)是重复操作。如下实例：
```python 实例(Python2.0+)
#!usr/bin/python
#-*- coding: UTF-8 -*-

str = 'Hello World!'

print str
print str[0]
print str[2:5]
print str[2:]
print str * 2   #输出字符串两次
print str + "TEST" #输出连接的字符串
```
Python 列表截取可以接收第三个参数，参数作用是截取的步长，以下实例在索引1到索引4的位置并设置为步长为2（间隔一个位置）来截取字符串：
![python3](/assets/python/python_list_slice_2.png)

## Python列表
列表可以完成大多数集合类的数据结构实现。支持字符，数字，字符串甚至包含列表(嵌套)。
列表用`[ ]`标识，是python最通用的负荷数据类型。
列表中值的切割也可以用到变量`[头下标:尾下标]`, 就可以截取相应的列表，从左到右索引默认0开始，从右到左索引默认 -1 开始，下标可以为空表示取到头或尾。
![list_slicing1_new1.png](/assets/python/list_slicing1_new1.png)

加号`+`是列表连接运算符，星号`*`是重复操作。如下实例：
```python 实例（Python2.0）
# !/usr/bin/python
#-*- coding: UTF-8 -*-

list = [ 'runoob', 786 , 2.23, 'john', 70.2]
tinylist = [123, 'john' ]
```

## Python 元组
元组是另一个数据类型，类似于List(列表).
元组用`()`标识。内部元素用逗号隔开。但是元组不能二次赋值，相当于只读列表。

```python 实例(Python2.0+)
#!/usr/bin/python
# -*- coding: UTF-8 *-*

tuple = ('runoob' , 786, 2.23, 'john', 70.2)
tinytuple = (123,'john')

print tuple
print tuple[0]
print tuple[1:3]
print tuple[2:]
print tinytuple *2
print tuple + tinytuple
```

[元组不允许更新，而列表允许更新!]{.warning .label}

## Python 字典
字典(dictionary)是除列表以外python之中最灵活的内置数据结构类型。列表是有序的对象集合，字典是无序的对象集合。
两者之间的区别在于：字典当中的元素是通过键来存储的，而不是通过偏移存取。
字典用"{}"标识。字典由索引(key)和它对应的值value组成。
```python 实例(Python 2.0+)
# !/usr/bin/python
# -*- coding: UTF-8 -*-

dict = {}
dict['one'] = "This is one"
dict[2] = "This is two"

tinydict = {'name': 'runoob', 'code': 6734, 'dept': 'sales'}

print dict['one']
print dict[2]           #输出键为2的值
print tinydict          #输出完整的字典
print tinydict.keys()   #输出所有键
print tinydict.values() #输出所有值
```
## Python 数据类型转换
|int(x[,base])|将x转化为一个整数|
|:------------|:---------------|
|long(x,[,base])|将x转化为一个长整型|
|float(x)|将x转换为一个浮点数|
|complex(real[,imag])|创建一个复数|
|str(x)|将对象x转换为字符串|
|repr(x)|将对象x转换为表达式字符串|
|eval(str)|用来计算在字符串中的有效Python表达式并返回一个对象|
|tuple(s)|将序列s转换为一个元组|
|list(s)|将序列s转换为一个列表|
|set(s)|转换为可变集合|
|dict(d)|创建一个字典，d必须是一个序列(key,value)元组|
|fronzenset(s)|转换为不可变集合|
|chr(x)|将一个整数转换为一个字符|
|unichr(x)|将一个整数转换为Unicode字符|
|ord(x)|将一个字符转换为它的整数值|
|hex(x)|将一个整数转换为一个十六进制字符串|
|oct(x)|将一个整数转换为八进制字符串|
