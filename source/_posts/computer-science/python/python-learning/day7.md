---
title: Python循环语句
date: 2022-03-03 15:17:39
categories:
 - [计算机科学,Python,Python基础教程]
tags: 
 - 编程技术
 - Python
---

# Python循环语句
循环语句允许我们执行一个语句或语句组多次
![loop](/assets/python/loop.png)

Python提供了for循环和while循环
[python中没有do..while循环]{.warning .label}
|循环类型|描述|
|:-------|:---|
|while循环|在给定的判断条件为true时执行循环体，否则退出循环体。|
|for循环|重复执行语句|
|嵌套循环|你可以在while循环体中嵌套for循环|

## 循环控制语句
循环控制语句可以更改语句执行的顺序。Python支持以下循环控制语句：
|控制语句|描述|
|:------|:---|
|break语句|在语句块执行过程中终止循环，并且跳出整个循环|
|continue语句|在语句块执行过程中终止当前循环，跳出该次循环，执行下一次循环|
|pass语句|pass是空语句，是为了保持程序结构的完整性|

### While循环语句

:::primary no-icon
while 判断条件(condition):
    执行语句(statements)...
:::

执行语句可以是单个语句或语句块。判断条件可以是任何表达式，任何非零、或非空(null)的值均为true。
当判断条件为false时，循环结束。
执行流程图如下：
![while-loop](/assets/python/while-loop.jpg)

while语句时还有另外两个重要的命令continue, break来跳过循环，continue用于跳出该次循环，break则是用于退出循环，此外“判断条件”，此外“判断条件”还可以是个常值，表示循环必定成立。
```python
# continue和break用法

i = 1
while i < 10
    i += 1
    if i%2 > 0: 
        continue
    print i

i = 1
while 1:
    print i
    i += 1
    if i > 10:
        break
```
#### 循环使用else语句
在python中，while...else在循环条件为false时执行else语句块:

```python while...else
#!/usr/bin/python

count = 0
while count < 5:
    print count, "is less than 5"
    count = count + 1
else: 
    print count, "is not less than 5"
```

### for循环语句
Python for循环可以遍历任何序列的项目，如一个列表或者一个字符串。
```python
for iterating_var in sequence
    statements(s)
```
![for-loop](/assets/python/for-loop.jpg)

```python for-loop
#!usr/bin/python
#-*- coding: UTF-8 -*-
for letter in 'Python': 
    print("当前字母: %s" %letter)

fruits = ['banana', 'apple', 'mango']
for fruit in fruits: 
    print ('当前水果: %s'% fruit)
print ("Good bye!")
```
#### 通过序列索引迭代
另外一种执行循环的遍历方式是通过索引

```python for索引
# !usr/bin/python
# -*- coding: UTF-8 -*-

fruits = ['banana', 'apple', 'mango']
for index in range(len(fruits)):
    print ('当前水果: %s' % fruits[index])

print ("Good bye!")
```
#### 循环使用else语句
在python中，for...else表示这样的意思，for中的语句和普通的没有区别，else中的语句会在循环正常执行完(即for不是通过break跳出而中断的)的情况下执行，while...else也是一样。
```python for-else
#!/usr/bin/python
# -*- coding: UTF-8 -*-

for num in range(10,20):
    for i in range(2,num):
        if num%i == 0:
            j = num/i
            print('%d等于 %d * %d' %(num,i,j))
            break
    else: 
        print ('%d 是一个质数' % num)
```


### Python break语句
break语句用来终止循环语句，即循环条件没有False条件或者序列还没被完全递归完，也会停止执行循环语句。
break语句用在while和for循环中。

### Python continue语句
Python continue语句跳出本次循环，而break跳出整个循环。
continue语句用来告诉Python跳出当前循环的剩余语句，然后继续进行下一轮循环。
continue语句用在while和for循环中。

### Python pass语句
Python pass是空语句，是为了保持程序结构的完整性。
pass不做任何事情，一般用做占位语句。
