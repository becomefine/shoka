---
title: Python中文编码
date: 2022-02-28 10:47:10
categories:
 - [计算机科学,Python,Python基础教程]
tags: 
 - 编程技术
 - Python
---

# Python中文编码
用Python输出"Hello,World"英文没有问题，但是如果你输出中文字符"你好，世界"就有可能会碰到中文编码问题。
Python文件中如果未指定编码，在执行过程会出现报错：
```python
#!/usr/bin/python
print("你好,世界")
```
以上程序执行输出结果为:
:::default no-icon
 File "test.py", line 2
SyntaxError: Non-ASCII character '\xe4' in file test.py on line 2, but no encoding declared; see http://www.python.org/peps/pep-0263.html for details
:::

Python中默认的1编码格式是ASCII格式，在没修改编码格式时无法正确打印汉字，所以在读取中文时会报错。
解决方法为只要在文件开头加入`# -*- config: UTF-8 -*-`或者 `# config=utf-8`就行了

[注意: # config=utf-8的=号两边不要空格]{.label .warning}
实例：
```python
#!/usr/bin/python
# -*- coding:UTF-8 -*-
pring("你好,世界")
```

输出结果为
:::default no-icon
你好，世界
:::

[注意：Python3.X源码默认使用utf-8编码，所以可以正常解析中文，无需指定UTF-8编码]{.label .waring}

