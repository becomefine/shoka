---
title: Python2.x与3.x版本区别
date: 2022-03-23 23:20:42
categories:
 - [计算机科学,Python,Python基础教程]
tags: 
 - 编程技术
 - Python
 - 面向对象
 - JSON
---

# Python2.x与3.x版本区别

Python 的 3​​.0 版本，常被称为 Python 3000，或简称 Py3k。相对于 Python 的早期版本，这是一个较大的升级。

为了不带入过多的累赘，Python 3.0 在设计的时候没有考虑向下相容。

许多针对早期 Python 版本设计的程式都无法在 Python 3.0 上正常执行。

为了照顾现有程式，Python 2.6 作为一个过渡版本，基本使用了 Python 2.x 的语法和库，同时考虑了向 Python 3.0 的迁移，允许使用部分 Python 3.0 的语法与函数。

新的 Python 程式建议使用 Python 3.0 版本的语法。

除非执行环境无法安装 Python 3.0 或者程式本身使用了不支援 Python 3.0 的第三方库。目前不支持 Python 3.0 的第三方库有 Twisted, py2exe, PIL等。

大多数第三方库都正在努力地相容 Python 3.0 版本。即使无法立即使用 Python 3.0，也建议编写相容 Python 3.0 版本的程式，然后使用 Python 2.6, Python 2.7 来执行。

Python 3.0 的变化主要在以下几个方面:

## print函数

print语句没有了，取而代之的是`print()`函数。Python2.6和Python2.7部分的支持这种形式的print语法。在Python2.6和Python2.7里面，以下三种形式是等价的：
```python
print "fish"
print ("fish") # 注意print后面有个空格
print("fish") # print()不能带有任何其它参数
```
然而，Python2.6实际上已经支持新的print()语法，实例:
```python
from __future__ import print_function
print("fish", "panda", sep=', ')
```

如果Python2.x版本想使用Python3.x的print函数，可以导入`__future__`包，该包禁用Python2.x的print语句，采用Python3.x的print函数：

## Unicode

Python 2 有 ASCII str() 类型，unicode() 是单独的，不是 byte 类型。

现在， 在 Python 3，我们最终有了 Unicode (utf-8) 字符串，以及一个字节类：byte 和 bytearrays。

由于 Python3.x 源码文件默认使用 utf-8 编码，所以使用中文就更加方便了：

## 除法运算

Python 中的除法较其它语言显得非常高端，有套很复杂的规则。Python 中的除法有两个运算符，/ 和 //

首先来说 / 除法:

在 Python 2.x 中 / 除法就跟我们熟悉的大多数语言，比如 Java 和 C ，整数相除的结果是一个整数，把小数部分完全忽略掉，浮点数除法会保留小数点的部分得到一个浮点数的结果。

在 Python 3.x 中 / 除法不再这么做了，对于整数之间的相除，结果也会是浮点数。

而对于 // 除法，这种除法叫做 floor 除法，会对除法的结果自动进行一个 floor 操作，在 Python 2.x 和 Python 3.x 中是一致的。
注意的是并不是舍弃小数部分，而是执行 floor 操作，如果要截取整数部分，那么需要使用 math 模块的 trunc 函数


## 异常

在 Python 3 中处理异常也轻微的改变了，在 Python 3 中我们现在使用 as 作为关键词。

捕获异常的语法由 except exc, var 改为 except exc as var。

使用语法except (exc1, exc2) as var 可以同时捕获多种类别的异常。 Python 2.6 已经支持这两种语法。

- 1. 在 2.x 时代，所有类型的对象都是可以被直接抛出的，在 3.x 时代，只有继承自BaseException的对象才可以被抛出。
- 2. 2.x raise 语句使用逗号将抛出对象类型和参数分开，3.x 取消了这种奇葩的写法，直接调用构造函数抛出对象即可。
在 2.x 时代，异常在代码中除了表示程序错误，还经常做一些普通控制结构应该做的事情，在 3.x 中可以看出，设计者让异常变的更加专一，只有在错误发生的情况才能去用异常捕获语句来处理。

## xrange

在 Python 2 中 xrange() 创建迭代对象的用法是非常流行的。比如： for 循环或者是列表/集合/字典推导式。

这个表现十分像生成器（比如。"惰性求值"）。但是这个 xrange-iterable 是无穷的，意味着你可以无限遍历。

由于它的惰性求值，如果你不得仅仅不遍历它一次，xrange() 函数 比 range() 更快（比如 for 循环）。尽管如此，对比迭代一次，不建议你重复迭代多次，因为生成器每次都从头开始。

在 Python 3 中，range() 是像 xrange() 那样实现以至于一个专门的 xrange() 函数都不再存在（在 Python 3 中 xrange() 会抛出命名异常）。

## 八进制字面量表示

八进制数必须写成0o777，原来的形式0777不能用了；二进制必须写成0b111。

新增了一个bin()函数用于将一个整数转换成二进制字串。 Python 2.6已经支持这两种语法。

在Python 3.x中，表示八进制字面量的方式只有一种，就是0o1000。

## 不等运算符

Python 2.x中不等于有两种写法 != 和 <>
Python 3.x中去掉了<>, 只有!=一种写法

## 去掉了repr表达式

Python 2.x 中反引号``相当于repr函数的作用

Python 3.x 中去掉了``这种写法，只允许使用repr函数，这样做的目的是为了使代码看上去更清晰么？不过我感觉用repr的机会很少，一般只在debug的时候才用，多数时候还是用str函数来用字符串描述对象。
```python
def sendMail(from_: str, to: str, title: str, body: str) -> bool:
    pass
```

## 数据类型

1）Py3.X去除了long类型，现在只有一种整型——int，但它的行为就像2.X版本的long
2）新增了bytes类型，对应于2.X版本的八位串，定义一个bytes字面量的方法如下：
str 对象和 bytes 对象可以使用 .encode() (str -> bytes) 或 .decode() (bytes -> str)方法相互转化。
3）dict的.keys()、.items 和.values()方法返回迭代器，而之前的iterkeys()等函数都被废弃。同时去掉的还有 dict.has_key()，用 in替代它吧 。


