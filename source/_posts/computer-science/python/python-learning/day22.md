---
title: Python正则表达式
date: 2022-03-18 17:28:47
categories:
 - [计算机科学,Python,Python基础教程]
tags: 
 - 编程技术
 - Python
 - 面向对象
---

# Python正则表达式

正则表达式是一个特殊的字符序列，能方便的检查一个字符串是否与某种模式匹配。
Python自1.5版本起增加了re模块，提供了Perl风格的正则表达式模式。
re模块使Python语言拥有全部的正则表达式功能。
compile函数根据一个模式字符串和可选的标志参数生产一个正则表达式对象。该对象拥有一系列方法用于正则表达式匹配和替换。
re模块也提供了与这些方法功能一致的函数，这些函数使用一个模式字符串作为他们的第一个参数。


## re.match函数

re.match尝试从字符串的起始位置匹配一个模式，如果不是起始位置匹配成功的话，match()就返回none。
```python re.match
re.match(pattern, string, flags = 0)
```
|参数|匹配的正则表达式|
|:---|:-------------|
|pattern|匹配的正则表达式|
|string|要匹配的字符串|
|flags|标志位，用于控制正则表达式的匹配方式，如是否匹配大小写，多行匹配等|

匹配成功re.match方法返回一个匹配的对象，否则返回None。
我们可以使用group(num)或groups()匹配对象函数来获取匹配表达式

|匹配对象方法|描述|
|group(num=0)|匹配的整个表达式的字符串，group()可以一次输入多个组号，在这种情况下他将返回一个包含那些组所对应的元组。|
|groups()|返回一个包含所有小组字符串的元组，从1到所含的小组号。|

```python
# !/usr/bin/python
# -*- coding: UTF-8 -*-

import re
print(re.match('www', 'www.runoob.com'),span()) #起始位置匹配
print(re.match('www', 'www.runoob.com'))        #不在起始位置匹配
```
:::default no-icon
(0, 3)
None
:::

```python
#!/usr/bin/python
import re
 
line = "Cats are smarter than dogs"
 
matchObj = re.match( r'(.*) are (.*?) .*', line, re.M|re.I)
 
if matchObj:
   print "matchObj.group() : ", matchObj.group()
   print "matchObj.group(1) : ", matchObj.group(1)
   print "matchObj.group(2) : ", matchObj.group(2)
else:
   print "No match!!"
```

:::default no-icon
matchObj.group() :  Cats are smarter than dogs
matchObj.group(1) :  Cats
matchObj.group(2) :  smarter
:::

## re.search方法

re.search扫描整个字符串并返回第一个成功的匹配。
语法:
:::default no-icon
re.search(pattern, string, flags = 0)
:::

|参数|描述|
|:---|:---|
|pattern|匹配的正则表达式|
|string|要匹配的字符串|
|flags|标志位，用于控制正则表达式的匹配方式|

匹配成功re.search方法返回一个匹配的对象，否则返回None。
我们可以使用group(num)或groups()匹配对象函数来获取匹配表达式。
|匹配对象方法|描述|
|group(num=0)|匹配的整个表达式的字符串，group()可以一次输入多个组号，在这种情况下将返回一个包含那些组所对应值的元组。|
|groups()|返回一个包含所有小组字符串的元组，从1到所含的小组号。|

```python
#!/usr/bin/python
# -*- coding: UTF-8 -*- 
 
import re
print(re.search('www', 'www.runoob.com').span())  # 在起始位置匹配
print(re.search('com', 'www.runoob.com').span())  # 不在起始位置匹配
```
:::default no-icon
(0, 3)
(11， 14)
:::

```python
#!/usr/bin/python
import re
 
line = "Cats are smarter than dogs";
 
searchObj = re.search( r'(.*) are (.*?) .*', line, re.M|re.I)
 
if searchObj:
   print "searchObj.group() : ", searchObj.group()
   print "searchObj.group(1) : ", searchObj.group(1)
   print "searchObj.group(2) : ", searchObj.group(2)
else:
   print "Nothing found!!"
```
:::default no-icon
searchObj.group() :  Cats are smarter than dogs
searchObj.group(1) :  Cats
searchObj.group(2) :  smarter
:::
-----------------

## re.match与re.search的区别
re.match只匹配字符串的开始，如果字符串开始不符合正则表达式，则匹配失败，函数返回None；而re.search匹配整个字符串，直到找到一个匹配。
```python
#!/usr/bin/python
import re
 
line = "Cats are smarter than dogs";
 
matchObj = re.match( r'dogs', line, re.M|re.I)
if matchObj:
   print "match --> matchObj.group() : ", matchObj.group()
else:
   print "No match!!"
 
matchObj = re.search( r'dogs', line, re.M|re.I)
if matchObj:
   print "search --> searchObj.group() : ", matchObj.group()
else:
   print "No match!!"
```

:::default no-icon
No match!!
search --> searchObj.group() : dogs
:::
------------

## 检索和替换

Python的re模块提供了re.sub用于替换字符串中的匹配项。
语法:
:::default no-icon
re.sub(pattern, repl, string, count = 0, flags = 0)
:::
参数:
- pattern: 正则中的模式字符串
- repl: 替换的字符串，也可为一个函数
- string: 要被查找替换的原始字符串。
- count: 模式匹配后替换的最大次数，默认0表示替换所有的匹配。

```python
#!/usr/bin/python
# -*- coding: UTF-8 -*-
 
import re
 
phone = "2004-959-559 # 这是一个国外电话号码"
 
# 删除字符串中的 Python注释 
num = re.sub(r'#.*$', "", phone)
print "电话号码是: ", num
 
# 删除非数字(-)的字符串 
num = re.sub(r'\D', "", phone)
print "电话号码是 : ", num
```
:::default no-icon
电话号码是: 2004-959-559
电话号码是: 2004959559
:::

### repl参数是一个参数

```python
# !/usr/bin/python
# -*- coding: UTF-8 -*-

import re

# 将匹配的数字乘以2
def double(matched):
    value = int(matched.group('value'))
    return str(value * 2)

s = 'A23G4HFD567'
print(re.sub('(?P<value>\d+)',double, s))
```
结果:
:::default no-icon
A46G8HFD1134
:::

### recompile函数

compile函数用于编译正则表达式，生产一个正则表达式(Pattern)对象，供match()和search()这两个函数使用:
:::defalut no-icon
re.compile(pattern[, flags])
:::
参数：
- `pattern`: 一个字符串形式的正则表达式
- `flags`: 可选，表示匹配模式，比如忽略大小写，多行模式等，具体参数为:
    1. **re.l**忽略大小写
    2. **re.L**表示特殊字符集\w,\W,\b,\B,\s,\S依赖于当前环境。
    3. **re.M**多行模式
    4. **re.S**即为`.`并且包括换行符在内的任意字符(`.`不包括换行符)
    5. **re.U**表示特殊字符集\w,\W,\b,\B,\d,\D,\s,\S依赖于Unicode字符属性数据库。
    6. **re.X**为了增加可读性，忽略空格和`#`后面的注释

```python
>>>import re
>>> pattern = re.compile(r'\d+')                    # 用于匹配至少一个数字
>>> m = pattern.match('one12twothree34four')        # 查找头部，没有匹配
>>> print m
None
>>> m = pattern.match('one12twothree34four', 2, 10) # 从'e'的位置开始匹配，没有匹配
>>> print m
None
>>> m = pattern.match('one12twothree34four', 3, 10) # 从'1'的位置开始匹配，正好匹配
>>> print m                                         # 返回一个 Match 对象
<_sre.SRE_Match object at 0x10a42aac0>
>>> m.group(0)   # 可省略 0
'12'
>>> m.start(0)   # 可省略 0
3
>>> m.end(0)     # 可省略 0
5
>>> m.span(0)    # 可省略 0
(3, 5)
```
当匹配成功时返回一个Match对象，其中:
- group([group1, ···])方法用于获得一个或多个分组匹配的字符串，当要获得整个匹配的子串时，可以直接使用group()或group(0);
- start([group])方法用于获取分组匹配的子串在整个字符串的起始位置(子串第一个字符的索引)，参数默认值为0；
- span([group])方法返回(start(group), end(group))。

### findall
在字符串中找到正则表达式所匹配的所有子串，并返回一个列表，如果有多个匹配模式，则返回元组列表，如果没有找到匹配的，则返回空列表。
**注意**: match和search是匹配一次findall匹配所有。
:::default no-icon
findall(string[, pos[, endpos]])

参数:
- `string`: 待匹配的字符串
- `pos`: 可选参数，指定字符串的起始位置，默认为0
- `endpos`: 可选参数，指定字符串的结束位置，默认为字符串的长度

```python 查找字符串中的所有数字
# -*- coding:UTF8 -*-
 
import re
 
pattern = re.compile(r'\d+')   # 查找数字
result1 = pattern.findall('runoob 123 google 456')
result2 = pattern.findall('run88oob123google456', 0, 10)
 
print(result1)
print(result2)
```
结果:
:::default no-icon
['123', '456']
['88', '12']
:::

多个匹配模式，返回元组列表:
```python
import re

result = re.findall(r'(\w+)=(\d+)', 'set width=20 and height=10')
print(result)
```
:::default no-icon
[('width', '20'), ('height', '10')]
:::

### re.finditer

和findall类似，在字符串中找到正则表达式所匹配的所有子串，并把他们作为一个迭代器返回。
:::default no-icon
re.finditer(pattern, string, flags=0)
:::

参数:
|参数|描述|
|:--|:---|
|pattern|匹配的正则表达式|
|string|要匹配的字符串|
|flags|标志位，用于控制正则表达式的匹配方式|

```python 实例
# -*- coding: UTF-8 -*-

import re

it = re.finditer(r"\d+", "12a32bc43jf3)
for match in it:
   print (match.group())
```
:::default no-icon
12
32
43
3
:::

### re.split

split方法按照能够匹配的子串将字符串分隔后返回列表，
语法:
:::default no-icon
re.split(pattern, string[, maxsplit=0, flags=0])
:::
参数：
|参数|描述|
|:---|:--|
|pattern|匹配的正则表达式|
|string|要匹配的字符串|
|maxsplit|分隔次数，maxsplit=1分隔一次，默认为0，不限制次数。|
|flags|标志位，用于控制正则表达式的匹配方式|

```python
>>>import re
>>> re.split('\W+', 'runoob, runoob, runoob.')
['runoob', 'runoob', 'runoob', '']
>>> re.split('(\W+)', ' runoob, runoob, runoob.') 
['', ' ', 'runoob', ', ', 'runoob', ', ', 'runoob', '.', '']
>>> re.split('\W+', ' runoob, runoob, runoob.', 1) 
['', 'runoob, runoob, runoob.']
 
>>> re.split('a*', 'hello world')   # 对于一个找不到匹配的字符串而言，split 不会对其作出分割
['hello world']
```

## 正则表达式对象

### re.RegexObject
re.compile()返回RegexObject对象

### re.MatchObject
group()返回被RE匹配的字符串。
- `start()`返回匹配开始的位置
- `end()`返回匹配结束的位置
- `span()`返回一个元组包含匹配(开始，结束)的位置
----------------------------

## 正则表达式修饰符-可选标志

正则表达式可以包含一些可选标志修饰符来控制匹配的模式。修饰符被指定为一个可选的标志。多个标志可以通过按位OR(|)他们来指定。如re.I|re.M被设置成I和M标志:

|修饰符|描述|
|:----|:----|
|re.I|使匹配对大小写不敏感|
|re.L|做本地化识别(locale-aware)匹配|
|re.M|多行匹配，影响^和$|
|re.S|使.匹配包括换行在内的所有字符|
|re.U|根据Unicode字符集解析字符。这个标志影响\w,\W,\b,\B.|
|re.X|该标志通过给予你更灵活的格式以便你将正则表达式写的更易于理解|

## 正则表达式模式

模式字符串是用特殊的语法来表示一个正则表达式:
字母和数字表示他们自身。一个正则表达式模式中的字母和数字匹配同样的字符串。
多数字母和数字前加一个反斜杠时会拥有不同的含义。
标点符号只有被转义时才匹配自身，否则他们表示特殊的含义。
反斜杠本身需要使用反斜杠转义。
由于正则表达式通常都包含反斜杠，所以你最好使用原始字符串来表示它们。模式元素(如r'\t', 等价于'\\t')匹配相应的特殊字符。
下表列出了正则表达式模式语法中的特殊元素。如果你使用模式的同时提供了可选的标志参数，某些模式元素的含义会改变。
|模式|描述|
|:---|:---|
|^|匹配字符串的开头|
|$|匹配字符串的末尾|
|.|匹配任意字符，除了换行符，当re.DOTALL标记被指定时，则可以匹配包括换行符的任意字符。|
|[...]|用来表示一组字符，单独列出: [amk]匹配'a', 'm'或'k'|
|[^...]|不在[]中的字符: [^abc]匹配除了a, b, c之外的字符。|
|re*|匹配0个或多个表达式|
|re+|匹配1个或多个表达式|
|re?|匹配0个或1个由前面的正则表达式定义的片段，非贪婪方式|
|re{n}|精确匹配n个前面表达式。例如，`o{2,}`不能匹配|
|re{n,}|匹配n个前面表达式。例如, o{2,}不能匹配"Bob"中的"o", 但是能匹配"food"中的两个o。|
|re{n, m}|匹配n到m次由前面的正则表达式定义的片段，贪婪方式|
|a\|b|匹配a或b|
|(re)|对正则表达式分组并记住匹配的文本|
|(?imx)|正则表达式包含三种可选标志: i, m, x。只影响括号中的区域。|
|(?-imx)|正则表达式关闭i,m,x可选标志。只影响括号中的区域。|
|(?: re)|类似(...), 但是不表示一个组|
|(?imx:re)|在括号中使用i,m,x可选标志|
|(?-imx:re)|在括号中不使用i,m,x可选标志|
|(?#...)|注释|
|(?=re)|前向肯定界定符。如果所含正则表达式，以...表示，在当前位置成功匹配时成功，否则失败。但一旦所含表达式已经尝试，匹配引擎根本没有提高；模式的剩余部分还要尝试界定符的右边。|
|?!re|前向否定界定符。与肯定界定符相反；当所含表达式不能在字符串当前位置匹配时成功。|
|?>re|匹配的独立模式，省去回溯。|
|\w|匹配字母数字即下划线|
|\W|匹配非字母数字及下划线|
|\s|匹配任意空白字符，等价于`[ \t\n\r\f]`|
|\S|匹配任意非空字符|
|\d|匹配任意数字，等价于[0-9]|
|\D|匹配任意非数字|
|\A|匹配字符串开始|
|\Z|匹配字符串结束，如果是存在换行，只匹配到换行前的结束字符串|
|\z|匹配字符串结束|
|\G|匹配最后匹配完成的位置|
|\b|匹配一个单词边界，也就是指单词和空格间的位置。例如, 'er\b'可以匹配"never"中的'er', 但不能匹配"verb"中的'er'。|
|\B|匹配非单词边界。'er\B'能匹配"verb"中的'er', 但不能匹配"never"中的'er'.|
|\n, \t等|匹配一个换行符。匹配一个制表符。等|
|\1...\9|匹配第n个分组的内容。|
|\10|匹配第n个分组的内容，如果它经匹配。否则指的是八进制字符码的表达式。|

## 正则表达式实例

**字符匹配**
|实例|描述|
|:---|:---|
|python|匹配"python".|

**字符类**
|实例|描述|
|:---|:---|
|[Pp]ython|匹配"Python"或"python"|
|rub[ye]|匹配"ruby"或"rube"|
|aeiou|匹配括号中的任意一个字母|
|[0-9]|匹配任何数字。类似于[0123456789]|
|[a-z]匹配任何小写字母|
|[A-Z]匹配任何大写字母|
|[a-zA-Z0-9]|匹配任何字母及数字|
|[^aeiou]|除了aeiou字母以外的所有字符|
|[^0-9]|匹配除了数字外的字符|

**特殊字符类**
|实例|描述|
|:---|:---|
|.|匹配除了"\n"之外的任何单个字符。要匹配包括'\n'在内的任何字符，请使用象'[.\n]'的模式|
|\d|匹配一个数字字符。等价于[0-9]|
|\D|匹配一个非数字字符。等价于[^0-9]|
|\s|匹配任何空白字符，包括空格、制表符、换页符等。等价于[\f\n\r\t\v]。|
|\S|匹配任何非空白字符，等价于[^\f\n\r\t\v]。|
|\w|匹配包括下划线的任何单词字符。等价于'[A-Za-z0-9_]'|
|\W|匹配任何非单词字符。等价于'[A-Za-z0-9_]'|
