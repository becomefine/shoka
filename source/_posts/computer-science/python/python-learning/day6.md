---
title: Python条件语句
date: 2022-03-03 14:34:31
categories:
 - [计算机科学,Python,Python基础教程]
tags: 
 - 编程技术
 - Python
---

# Python条件语句
条件语句是通过一条或多条语句的执行结果(True或False)来决定执行的代码块。
![if-condition](/assets/python/if-condition.jpg)
Python程序语言指定任何非0和费控(null)值为null, 0或者null为false。
Python编程中if语句用于控制程序的执行，基本形式为

:::default no-icon
if 判断语句
    执行语句...
else:
    执行语句...
:::

:::warning 
由于python并不支持switch语句，所以多个条件判断智能用elif来实现，如果判断需要多个条件需同时判断时，可以使用or(或)，表示两个条件有一个成立时判断条件成功; 使用and(与)时，表示只有两个条件同时成立的情况下，判断条件才成功。
:::
