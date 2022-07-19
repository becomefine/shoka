---
title: Python字符串
date: 2022-03-08 20:50:00
categories:
 - [计算机科学,Python,Python基础教程]
tags: 
 - 编程技术
 - Python
---

# Python字符串

```python 定义字符串
var1 = 'Hello World!'
var2 = "Python Runoob"
```

## Python访问字符串中的值
Python不支持单字符串类型，单字符串在Python中也是作为一个字符串使用。
Python访问子字符串，可以使用方括号来截取字符串。

```python 
#!/usr/bin/python
var1 = 'Hello World!'
var2 = "Python Runoob"

print "var1[0]: ", var1[0]
print "var2[1:5] ", var2[1:5]
```
结果：
:::default no-icon
var1[0]: H
var2[1:5]: ytho
:::

## Python转义字符
在需要在字符中使用特殊字符时，python用反斜杠`\`转义字符。
|转义字符|描述|
|:-------|:---|
|\\(在行尾处)|续行符|
|\\\\  |反斜杠符号|
|\\'  |单引号|
|\\"|双引号|
|\\a|响铃|
|\\b|退格(Backspace)|
|\\e|转义|
|\\000|空|
|\\n|换行|
|\\v|纵向制表符|
|\\t|横向制表符|
|\\oyy|八进制数, y代表0-7的字符, 例如: \\012代表换行|
|\\xyy|十六进制数，以\\x开头，yy代表的字符，例如: \\x0a代表换行|
|\\other|其他的字符以普通格式输出|

## Python 字符串运算符
下表实例变量a值为字符串"Hello", b变量为"Python"
|操作符|描述|实例|
|:-----|:--|:-----|
|+|字符串连接|>>>a + b 'HelloPython'|
|*|重复输出字符串|>>>a * 2  'HelloHello'|
|[] |通过索引获取字符串中字符|>>>a[1]  'e'|
|[:]|截取字符串中的一部分|>>>a[1:4]  'ell'|
|in |成员运算符-如果字符串中包含给定的字符返回True|>>>"H" in a  True|
|not in|成员运算符-如果字符串中不包含给定的字符返回True|>>>"M" not in a   True|
|r/R|原始字符串-原始字符串: 所有的字符串都是直接按照字面的意思来使用，没有转义特殊或不能打印的字符。原始字符串除在字符串的第一个引号前加上字母"r"(可以大小写)以外，与普通字符串有着几乎完全相同的语法。|>>>print r'\n' \\n >>>print R'\n' \n|
|% |格式字符串||

## Python字符串格式化
Python支持格式化字符串的输入。尽管这样可能会用到非常复杂的表达式，但最基本的用法是将一个值插入到一个有字符串格式符%s的字符串中。
在Python中，字符串格式化使用与C中sprintf函数一样的语法。
```python
#!/usr/bin/python
print "My name is %s and weight is %kg! %('Zara', 21)
```
以上实例输入结果：
:::default no-icon
My name is Zara and Weight is 21 kg!
:::

|符号| 描述|
|:---|:----|
|%c|格式化字符及其ASCII码|
|%s|格式化字符串|
|%u|格式化整数|
|%o|格式化无符号|
|%x|格式化无符号十六进制数|
|%X|格式化无符号十六进制数(大写)|
|%f|格式化浮点数字，可指定小数点后的精度|
|%e|用科学计数法格式化浮点数|
|%E|作用同%e，用科学计数法格式化浮点数|
|%g|%f和%e的简写|
|%G|%F 和 %E的简写|
|%p|用十六进制数格式化变量的地址|

格式化操作符辅助指令
|符号|功能|
|:--|:----|
|*|定义宽度或者小数点精度|
|-|用做左对齐|
|+|在正数前面显示加号(+)|
|<sp>|在正数前面显示空格|
|# |在八进制数前面显示零('0'), 在十六进制前面显示'0x'或者'0X'(取决于用的是'x'还是'X')|
|0|显示的数学前面填充'0'而不是默认的空格|
|%|'%%'输出一个单一的'%'|
|(var)|映射变量(字典参数)|
|m.n.|m是显示的最小总宽度, n是小数点后的位数(如果可用的话)|

## Python三引号
Python 中三引号可以将复杂的字符串进行赋值。
Python三引号允许一个字符串跨多行，字符串中可以包含换行符、制表符以及其他特殊字符。
三引号的语法是一对连续的单引号或者双引号(通常都是成对的用)。
:::default no-icon
\>>> hi = '''hi
there '''
\>>> hi # repr()
'hi\nthere'
\>>> print hi # str()
hi
there
:::

一个典型的用例是, 当你需要一块HTML或者SQL时, 这时当用三引号标记, 使用传统的转义字符体系将十分费神。

## Unicode字符串
Python中定义一个Unicode字符串和定义一个普通字符串一样简单：
:::default no-icon
\>>> u'Hello World !'
u'Hello World !'
:::
被替换的\u0020标识表示在给定位置插入编码值为0x0020的Unicode字符(空格符)。

## python的字符串内建函数
字符串方法是从python1.6到2.0慢慢加进来的————它们也被加到了Jython中。
这些方法实现了string模块的大部分方法，如下表所示列出目前字符串内建支持的方法。
|方法|描述|
|:---|:---|
|string.captitalize()|把字符串的第一个字符大写|
|string.center(width)|返回一个原字符串居中，并使用空格填充至长度width的新字符串|
|string.count(str, beg=0, end=len(string))|返回str在string里面出现的次数，如果beg或者end指定则返回指定范围内str出现的次数|
|string.decode(encoding='UTF-8', errors='strict')|以encoding指定的编码格式解码string, 如果出错默认报一个ValueError的异常, 除非errors指定的'ignore'或者'replace'|
|string.endswidth(obj, beg=0, end=len(string))|检查字符串是否以obj结束，如果beg或者end指定则检查指定的范围内是否以obj结束, 如果是， 返回True， 否则返回False。|
|string.expandtabs(tabsize=8)|把字符串string中的tab符号转为空格，tab符号默认的空格数是8。|
|string.find(str, beg=0, end=len(string))|检测str是否包含在string中，如果beg和end指定范围，则检查是否包含在指定范围内，如果是返回开始的索引值，否者返回-1|
|string.format()|格式化字符串|
|string.index(str,beg=0,end=len(string))|跟find()方法一样，只不过如果str不在string中会报一个一样|
|string.isalnum()|如果string至少有一个字符并且所有字符都是字母或数字则返回true，否则返回False|
|string.isalpha()|如果string至少有一个字符并且所有字符都是字母则返回True，否则返回False|
|string.isdecimal()|如果string只包含十进制数字则返回True否则返回False|
|string.isdigit()|如果string只包含数字则返回True否则返回False|
|string.islower()|如果string中包含至少一个区分大小写的字符，并且所有这些(区分大小写的)字符都是小写，则返回True, 否则返回False。|
|string.isnumeric()|如果string中只包含数字字符，则返回True，否则返回False|
|string.isspace()|如果string中只包含空格，则返回True，否则返回False|
|string.istitle()|如果string是标题化的则返回True，否则返回False|
|string.isupper()|如果string中包含至少一个区分大小写的字符，并且所有这些字符都是大写，则返回True，否则返回False。|
|string.join(seq)|以string作为分隔符，将seq中所有的元素(的字符串表示)合并为一个新的字符串|
|string.ljust(width)|返回一个原字符串左对齐，并使用空格填充至长度width的新字符串|
|string.lower()|转换string中所有大写字符为小写|
|string.lstrip()|截掉string左边的空格|
|string.maketrans(intab, outtab])|makestrans()方法用于创建字符映射的转换表，对于接受两个参数的最简单的调用方式，第一个参数是字符串，表示需要转换的字符，第二个参数也是字符串表示转换的目标。|
|max(str)|返回字符串str中最大的字母|
|min(str)|返回字符串str中最小的字母|
|string.partition(str)|有点像find()和split()的结合体，从str出现的第一个位置起，把字符串string分成衣蛾3元素的元组(string_pre_str,str,string_post_str),如果string中不包含str则string_pre_str == string.|
|string.replace(str1,str2,num=string.count(str1))|把string中的str1替换为str2，如果num指定，则替换不超过num次|
|string.rfind(str,beg=0,end=len(string))|类似于find()函数，返回字符串最后一次出现的位置，如果没有匹配项则返回-1|
|string.rindex(str,beg=0,end=len(string))|类似于index()，不过是返回最后一个匹配到的子字符串的索引号。|
|string.rjust(widht)|返回一个原字符串右对齐，并使用空格填充至长度width的新字符串。|
|string.rpartition(str)|类似于parition()函数，不过是从右边开始查找。|
|string.rstrip()|删除string字符串末尾的空格。|
|string.split(str="",num=string.count(str))|以str为分隔符切片string, 如果num有指定值，则仅分隔num+1个字符串|
|string.splitlines([keepends])|按照行('\r','\r\n','\n')分隔，返回一个包含各行作为元素的列表，如果参数keepends为False，不包含换行符，如果为True，则保留换行符。|
|string.startswith(obj,beg=0,end=len(string))|检查字符串是否是以obj开头，是则返回True，否则返回False。如果beg和end指定值，则在指定范围内检查。|
|string.strip([obj])|在string上执行lstrip()和rstrip()。|
|string.swapcase()|翻转string中的大小写。|
|string.title()|返回"标题化"的string, 就是说所有单词都是以大写开始，其余字母均为小写(见istitle())。|
|string.translate(str,del="")|根据str给出的表(包含256个字符)转换string的字符，要过滤掉的字符放到del参数中。|
|string.upper()|转换string中的小写字母为大写。|
|string.zfill(width)|返回长度为width的字符串, 原字符串stirng()右对齐，前面填充0|
