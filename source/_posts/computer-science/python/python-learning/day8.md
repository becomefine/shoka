---
title: Pythonn数字
date: 2022-03-08 18:03:57
categories:
 - [计算机科学,Python,Python基础教程]
tags: 
 - 编程技术
 - Python
---

# Python Number
Python数据类型用于存储数值。
```python
var1 = 1
var2 = 10
```

## 数值类型:
- 整形(Int)-通常被称为整型或整数，是正或负整数，不带小数点。
- 长整型(long integers)-无限大小的整数，整数最后是一个大写或小写的L。
- 浮点型(floating point real values)-浮点型由整数部分与小数部分组成，浮点型也可以使用科学计数表示。
- 复数(complex numbers)-复数由实数部分和虚数部分构成。

## Python Number类型转换
|函数|描述|
|:---|:---|
|int(x [,base])|将x转换为一个整数|
|long(x [,base])|将x转换为一个长整型|
|float(x)|将x转换为一个浮点数|
|complex(real [,imag])|创建一个复数|
|str(x)|将对象x转换为字符串|
|repr(x)|将对象x转换为表达式字符串|
|eval(str)|用来计算在字符串中的有效Python表达式，并返回一个对象|
|tuple(s)|将序列s转换为一个元组|
|list(s)|将序列s转换为一个列表|
|chr(x)|将一个整数转换为一个字符|
|unichr(x)|将一个整数转换为unicode字符|
|ord(x)|将一个字符转换为它的整数值|
|hex(x)|将一个整数转换为一个十六进制字符串|
|oct(x)|将一个整数转换为一个八进制字符串|

## Python math模块、cmath模块
Python math模块提供了许多对浮点数的数学运算函数
Python cmath模块包含了一些用于复数运算的函数
cmath模块的函数跟math模块函数基本一致，区别是cmath模块运算的是复数，math模块运算的是数学运算。
```python 导入math或cmath
import math
```
|函数|返回值|
|:---|:----|
|abs(x)|返回数值的绝对值|
|ceil(x)|上入整数|
|cmp(x,y)|x小于y小于返回-1，等于返回0，大于返回1|  
|exp(x)|返回e的x次幂|
|fabs(x)|返回数值的绝对值|
|floor(x)|返回数字的下舍整数|
|log(x)|如math.log(math.e)返回1.0|
|log10(x)|返回以10为技术的x的对数。|
|max(x1,x2...)|返回给定参数的最大值|
|min(x1,x2...)|返回给定参数的最小值|
|modf(x)|返回x的整数部分与小数部分，两部分的数值符号与x相同，整数部分以浮点型表示|
|pow(x,y)|x**y运算后的值|
|round(x[,n])|返回浮点数x的四舍五入值，如给出n值，则代表舍入到小数点后的位数|
|sqrt(x)|返回数字x的平方根|

## Python 随机数函数
|函数|描述|
|:---|:----|
|choice(seq)|从序列的元素中随机挑选一个元素，比如random.choice(range(10)), 从0-9中随机挑选一个整数|
|randrange([start,] stop[,step])|从指定范围内，按指定基数递增的集合中获取一个随机数，基数默认值为1|
|random()|随机生产下一个实数，它在[0,1)范围内。|
|seed([x])|改变随机数生成器的种子seed|
|shuffle(lst)|将序列的所有元素随机排序|
|uniform(x, y)|随机生成下一个实数，它在[x,y]范围内|

## Python三角函数

|函数|描述|
|:---|:---|
|acos(x)|返回x的反余弦弧度值。|
|asin(x)|返回x的反正弦弧度值。|
|atan(x)|返回x的反正切弧度值。|
|atan2(y,x)|返回给定的X及Y坐标值的反正切值|
|cos(x)|返回x的弧度的余弦值|
|hypot(x,y)|返回欧几里得范数sqrt(x*x+y*y)|
|sin(x)|返回x弧度的正弦值|
|tan(x)|返回x弧度的正切值|
|degrees(x)|将弧度转换为角度, 如degrees(math.pi/2)，返回90.0|
|radians(x)|将角度转换为弧度|

## Python 数学常量
|常数|描述|
|:---|:---|
|pi|数学常量pi(圆周率，一般以\pi来表示)|
|e |数学常量e, e即自然常数|
