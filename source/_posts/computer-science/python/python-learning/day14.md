---
title: Python函数
date: 2022-03-10 22:06:49
categories:
 - [计算机科学,Python,Python基础教程]
tags: 
 - 编程技术
 - python
---

# Python函数
函数是组织好的，可重复使用的，用来实现单一，或相关联功能的代码段。
函数能提高应用的模块性，和代码的重复利用率。

## 定义一个函数
- 函数代码以`def`关键字开头，后接函数标识符名称和圆括号()。
- 任何传入参数和自变量必须放在圆括号中间。圆括号之间可以用于定义参数。
- 函数的第一行语句可以选择性的使用文档字符串-用于存放函数说明。
- 函数内容以冒号起始，并且缩进。
- return[表达式]结束函数，选择性地返回一个值给调用方。不带表达式的return相当于返回None。

```python 示例
def functionname(parameters):
    "函数_文档字符串"
    function_suite
    return [expression]
```
默认情况下，参数值和参数名称是按函数声明中定义的顺序匹配起来的。

## 函数调用
定义一个函数只给了函数一个名称，指定了函数里包含的参数，和代码块结构。
这个函数的基本结构完成以后，可以通过另一个函数调用执行，也可以直接从Python提示符执行。

## 参数传递
在python中，类型属于对象，变量是没有类型的:
```python 
a = [1, 2, 3]
a = "Runoob"
```
以上代码中，[1,2,3]是List类型，"Runoob"是String类型，而变量a是没有类型，仅仅是一个对象的引用(一个指针)，可以是List类型对象，也可以指向String类型对象。

## 可更改(mutable)与不可更改(immutable)对象
在python中，strings, tuples, 和numbers是不可更改对象，而list, dict等则是可以修改的对象。
- **不可变类型**: 变量赋值`a=5`后再赋值`a=10`，这里实际是新生成一个int值对象10，再让a指向它，而5被丢弃，不是改变a的值，相当于新生成了a。
- **可变类型**: 变量赋值`la=[1,2,3,4]`后再赋值`la[2]=5`则是将list la的第三个元素值更改，本身la没有动，只是其内部的一部分值被修改了。

python函数的参数传递:
- **不可变类型**: 类似c++的值传递，如整数、字符串、元组。如fun(a), 传递的只是a的值，并没有影响a对象本身。比如在fun(a)内部修改a的值，只是修改另一个复制的对象，不会影响a本身。
- **可变类型**: 类似c++的引用传递，如列表，字典。如fun(la)，则是将la真正的传过去，修改后fun外部的la也会受影响。

python中一切对象、严格意义我们不能说值传递还是引用传递，我们应该说传不可变对象和传可变对象。

**python传不可变对象实例**
```python
# !/usr/bin/python
# -*- coding: UTF-8 -*-
def ChangeInt(a):
    a = 10

b = 2
changeInt(b)
print b #结果是2
```
实例中有int对象2，指向它的变量是b，在传递给ChangeInt函数时，按传值的方式复制了变量b，a和b都指向了同一个Int对象，在a=10时，则新生成一个int对象10，并让a指向它。

**python传可变对象实例**
```python 
# !/usr/bin/python
# -*- coding: UTF-8 -*-

# 可写函数说明
def changeme(mylist):
    "修改传入的列表"
    mylist.append([1,2,3,4])
    print "函数内取值: ", mylist
    return

# 调用changeme函数
mylist = [10, 20, 30]
changeme( mylist )
print "函数外取值: ", mylist
```

实例中传入函数的和在末尾添加新内容的对象用的是同一个引用，结果:
:::default no-icon
函数内取值: [10, 20, 30, [1,2,3,4]]
函数内取值: [10, 20, 30, [1,2,3,4]]
:::
---------------------------------------

## 参数
以上是调用函数时可使用的正式参数类型:
- 必备参数
- 关键字参数
- 默认参数
- 不定长参数

### 必备参数
必备参数须以正确的顺序传入函数。调用时的数量必须和声明时的一样。调用printme()函数，你必须传入一个参数，不然会出现语法错误:

```python
# !/usr/bin/python
# -*- coding: UTF-8 -*-

#可写函数说明
def printme( str )
    "打印任何传入的字符串"
    print str
    return 

#调用printme函数
printme() #报错
```

### 关键字参数
关键字参数和函数调用关系紧密，函数调用使用关键字参数来确定传入的参数值。
使用关键字参数允许函数调用时参数的顺序与声明时不一样，因为Python解释器能够用参数名匹配参数值。
```python
# !/usr/bin/python
# -*- coding: UTF-8 -*-

#可写函数说明
def printme( str )
    "打印任何传入的字符串"
    print str
    return

#调用printme函数
print( str = "My string" )
```

### 默认参数
调用函数时，默认参数的值如果没有传入，则被认为是默认值。

```python
# !/usr/bin/python
# -*- coding: UTF-8 -*-

#可写函数说明
def printinfo(name, age = 35)
    "打印任何传入的字符串"
    print "Name: ", name
    print "Age ", age
    return

# 调用printinfo函数
printinfo( age=50, name = "miki" )
printinfo( name = "miki" )
```

### 不定长参数
可能需要一个函数能处理比当初声明时更多的参数。这些参数叫做不定长参数，和上述2种参数不同，声明时不会命名。
```python
def functionname([formal_args,] *var_args_tuple):
    "函数_文档字符串"
    function_suite
    return [expression]
```
加了星号(*)的变量名会存放所有未命名的变量参数。

```python 不定长参数实例
# ！usr/bin/python
# -*- coding: UTF-8 -*-

# 可写函数说明
def printinfo( arg1, *vartuple ):
    "打印任何传入的参数"
    print "输出: "
    print arg1
    for var in vartuple:
        print var
    return

# 调用printinfo函数
printinfo(10)
printinfo(70, 60, 50)
```

## 匿名函数
python使用lambda来创建匿名函数
- lambda只是一个表达式，函数体比def简单很多。
- lambda的主体是一个表达式，而不是一个代码块。仅仅能在lambda表达式中封装有限的逻辑进去。
- lambda函数拥有自己的命名空间，且不能访问自有参数列表之外或全局命名空间里的参数。
- 虽然lambda函数看起来只能写一行，却不等同于C或C++的内联函数，后者的目的是调用小涵书时不占用栈内存从而增加运行效率。

### 语法
lambda函数的语法只包含一个语句。
:::default no-icon
lambda [arg1 [,arg2,......argn]]:expression
:::

```python lambda表达式
# !/usr/bin/python
# -*- coding: UTF-8 -*-

#可写函数说明
sum = lambda arg1, arg2: arg1 + arg2

# 调用sum函数
print "相加后的值为: ", sum(10, 20)
print "相加后的值为: ", sum(20, 20)
```
***

## return语句

return语句[表达式]退出函数，选择性地向调用方返回一个表达式。不带参数值的return语句返回None。
```python
# !/usr/bin/python
#  -*- coding: UTF-8 -*-

# 可写函数说明
def sum(arg1, arg2):
    #返回2个参数的和."
    total = arg1 + arg2
    print "函数内: ", total
    return total

# 调用sum函数
total = sum(10, 20)
```

## 变量作用域

一个程序的所有的变量并不是在哪个位置都可以访问的。访问权限决定于这个变量是在哪里赋值的。变量的作用域决定了在哪一部分程序你可以访问哪个特定的变量名称。两种最基本的变量作用域如下:
- 全局变量
- 局部变量

### 全局变量和局部变量
定义在函数内部的变量拥有一个局部作用域，定义在函数外的拥有全局作用域。
局部变量只能在其被声明的函数内部访问，而全局变量可以在整个程序范围内访问。调用函数时，所有在函数内声明的变量名称都将被加入到作用域中。

```python
# !/usr/bin/python
# -*- coding: UTF-8 -*-

total = 0 #这是一个全局变量
# 可写函数说明
def sum(arg1, arg2)
    #返回2个参数的和."
    total = arg1 + arg2 #total在这里是局部变量.
    print "函数内部是局部变量: ", total
    return total

#调用sum函数
sum(10, 20)
print "函数外是全局变量: ", total
```

