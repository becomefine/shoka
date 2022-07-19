---
title: Python JSON
date: 2022-03-23 17:44:55
categories:
 - [计算机科学,Python,Python基础教程]
tags: 
 - 编程技术
 - Python
 - 面向对象
 - JSON
---

# Python JSON

## JSON函数

使用JSON函数需要导入json库：import json。
|函数|描述|
|:--|:---|
|json.dumps|将Python对象编码成JSON字符串|
|json.loads|将已编码的JSON字符串解码为Python对象|

## json.dumps

json.dumps用于将Python对象编码成JSON字符串。
```python json.dumps
json.dumps(obj, skipkeys=False, ensure_ascii=True, check_circular=True, allow_nan=True, cls=None, indent=None, separators=None, encoding="utf-8", default=None, sort_keys=False, **kw)
```

实例:
```python 将数组编码为JSON格式数据
#!/usr/bin/python
import json

data = [ { 'a' : 1, 'b' : 2, 'c' : 3, 'd' : 4, 'e' : 5 } ]

data2 = json.dumps(data)
print(data2)
```
使用参数让JSON数据格式化输出:

```python
# !/usr/bin/python
import json

data = [{'a': 1, 'b': 2, 'c': 3, 'd': 4, 'e': 5}]

data2 = json.dumps({'a': 'Runoob', 'b': 7}, sort_keys=True, indent=4, separators=(',', ': '))
print(data2)
```
结果:
:::default no-icon
{
    "a": "runoob',
    "b": 7
}

python原始类型向json类型的转化对照表:
|Python|JSON|
|:-----|:---|
|dict|object|
|list, tuple|array|
|str, unicode|string|
|int, long, float|number|
|True|true|
|False|false|
|None|null|

## json.loads

json.loads用于解码JSON数据。该函数返回Python字段的数据类型。
**语法**:
```python
json.loads(s[, encoding[, cls[, object_hook[, parse_float[, parse_int[, parse_constant[, object_pairs_hook[, **kw]]]]]]]])
```

**实例**:
```python
# !/usr/bin/python
import json

jsonData = '{"a": 1, "b": 2, "c":3, "d": 4, "e": 5}';

text = json.loads(jsonData)
print(text)
```
结果:
:::default no-icon
{u'a': 1, u'c': 3, u'b': 2, u'e': 5, u'd': 4}
:::

json类型转换到python的类型对照表:

|JSON|Python|
|:---|:-----|
|object|dict|
|array|list|
|string|unicode|
|number(int)|int, long|
|number(ral)|float|
|true|True|
|false|False|
|null|None|

更多内容: https://docs.python.org/2/library/json.html。

## 使用第三方库: Demjson

Demjson是python的第三方模块库，可用于编码和解码JSON数据，包含了JSONLint的格式化及校验功能。
Github地址: https://github.com/dmeranda/demjson
官方地址: http://deron.meranda.us/python/demjson/

### 环境配置

在使用Demjson编码或解码JSON数据前，我们需要先安装Demjson模块。
[Demjson](http://deron.meranda.us/python/demjson/download)

```bash
$ tar -xvzf demjson-2.2.3.tar.gz
$ cd demjson-2.2.3
$ python setup.py install
```
更多安装介绍: http://deron.meranda.us/python/demjson/install

## JSON 函数

|函数|描述|
|:---|:---|
|encode|将Python对象编码成JSON字符串|
|decode|将已编码的JSON字符串解码为Python对象|

### encode

Python encode()函数用于将Python对象编码成JSON字符串。

**语法**:
```python
demjson.encode(self, obj, nest_level=0)
```
**实例**:
将数组编码为JSON格式数据:

```python
#!/usr/bin/python
import demjson

data = [ { 'a' : 1, 'b' : 2, 'c' : 3, 'd' : 4, 'e' : 5 } ]

json = demjson.encode(data)
print(json)
```
结果:
:::default no-icon
[{"a":1,"b":2,"c":3,"d":4,"e":5}]
:::

### decode

Python 可以使用demjson.decode()函数解码JSON数据。该函数返回Python字段的数据类型。
**语法**:
```python
demjson.decode(self, txt)
```
```python
#!/usr/bin/python
import demjson

json = '{"a": 1, "b": 2, "c": 3, "d": 4, "e": 5}';

text = demjson.decode(json)
print(text)
```
结果:
:::default no-icon
{u'a': 1, u'c': 3, u'b': 2, u'e': 5, u'd': 4}
:::



