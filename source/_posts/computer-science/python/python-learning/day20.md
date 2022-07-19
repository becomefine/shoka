---
title: 内置函数
date: 2022-03-14 08:44:56
categories:
 - [计算机科学,Python,Python基础教程]
tags: 
 - 编程技术
 - Python
---

# Python 内置函数
|内置函数|描述|调用方式|备注|
|:------|:---|:------|:---|
|abs()|返回数字的绝对值|abs(x)|x:数值表达式|
|all()|判断给定的可迭代参数iterable中的所有元素是否为TRUE，如果是返回True，否则返回False。元素除了是0、空、None、False外都算True|all(iterable)|iterable: 元组或列表。|
|any()|判断给定的可迭代参数iterable是否全部为False，则返回False，如果有一个为True，则返回True，元素除了是0、空、False外都算TRUE。|iterable: 元组或列表|
|basestring()|判断一个对象是否为str或者unicode的实例。|basestring()| | |
|bin()|返回一个整数int或者长整数long int的二进制表示|bin(x)|x: int或者long int数字|
|bool()|用于将给定参数转换为布尔类型，如果没有参数，返回False。|bool([x])|x: 要进行转换的参数.|
|bytearray()|返回一个新字节数组。这个数组里的元素是可变的。|bytearray([source[,encoding[, errors]]])|source为整数则返回一个长度为source的初始化数组; 为字符串则按照指定的encoding将字符串转换为字节序列; 为可迭代类型则元素必须为[0, 255]中的整数; 为与buffer接口一致的对象则此对象也可以被用于初始化bytearray; 没有任何输入参数，默认就是初始化数组为0个元素。|
|callable()|用于检查一个对象是否是可调用的。如果返回True, object仍然可能调用失败; 但如果返回False, 调用对象object绝对不会成功。|callable(object)|object: 对象|
|chr()|用一个范围在range(256)内的整数作参数，返回一个对应的字符。|chr(i)|i: 可以是10进制也可以是16进制形式的数字。|
|classmethod|classmethod修饰符对应的函数不需要实例化，不需要self参数，但第一个参数需要是表示自身类的cls参数，可以来调用类的属性，类的方法，实例化对象等。|classmethod|无|
|cmp()|比较2个对象，如果x < y返回-1，如果x == y返回0， 如果x > y返回1.|cmp(x,y)|x: 数值表达式。y: 数值表达式|
|compile()|将一个字符串编译成字节代码。|compile(source, filename, mode[,flags[, dont_inherit]])|source: 字符串或者AST对象。filename: 代码文件名称。mode: 指定编译代码的种类。flags: 变量做作用域。flags和dont_inherit用来控制编译源码时的标志。|
|complex()|用于创建一个值为real + imag*j的复数或者转化一个字符串或数为复数。如果第一个参数为字符串，则不需要指定第二个参数。|complex([real[, imag]])|real: int, long, float或字符串。imag: int, long, float|
|delattr()|用于删除属性|delattr(object, name)|object: 对象。name: 必须是对象的属性。|
|dict()|创建一个字典|dict(**kwarg), dict(mapping, **kwarg), dict(iterable, **kwarg)| **kwargs: 关键字，mapping: 元素的容器，映射类型(Mapping Types)是一种关联式的容器类型，存储了对象与对象之间的映射关系。iterable: 可迭代对象。|
|dir()|不带参数时，返回当前范围内的变量、方法和定义的类型列表; 带参数是，返回参数的属性、方法列表。如果参数包含方法_dir_()，该方法将被调用。如果参数不包含_dir_()，该方法将最大限度地收集参数信息。|dir([object])|object: 对象、变量、类型。|
|divmod()|把除数和余数运算结果结合起来，返回一个包含商和余数的元组(a//b, a%b).|divmod(a, b)|a:数字，b: 数字|
|enumerate()函数|用于将一个可遍历的数据对象(如列表、元组或字符串)组合为一个索引序列，同时列出数据和数据下标，一般用在for循环当中。|enumerate(sequence, [start=0])|sequence: 一个序列、迭代器或其他支持迭代对象。start:下标起始位置的值。|
|eval()|执行一个字符串表达式，并返回表达式的值。|eval(expression[, globals[, locals]])|expression: 表达式，globals: 变量作用域，全局命名空间，必须是一个字典对象。locals: 变量作用域，局部命名空间，可以是任何映射对象。|
|execfile()|用来执行一个文件|execfile(filename[, globals[, locals]])|filename: 文件名。globals: 变量作用域, 全局命名空间，字典对象。locals: 变量作用域，局部命名空间，可以映射任何映射对象。|
|file()|创建一个file对象，有一个别名open()。参数以字符串形式传递。|file(name[, mode[, buffering]])|name: 文件名，mode: 打开模式，buffering: 0表示不缓冲，1表示进行行缓冲，大于1为缓冲大小。|
|filter()|用于过滤序列，过滤掉不符合条件的元素，返回由负荷条件元素组成的新列表。|filter(function, iterable)|function: 判断函数。iterable: 可迭代对象。|
|float()|用于将整数和字符串转换成浮点数。|float([x])|x: 整数或字符串|
|format()|格式化对象|str.format()||
|frozenset()|返回一个冻结的集合，冻结后集合不能再添加或删除任何元素。|frozenset([iterable])|iterable: 可迭代的对象，比如列表、字典、元组等|
|getattr()|返回一个对象属性值。|getattr(object, name[, default])|object: 对象。name: 字符串，对象属性。default: 默认返回值，如果不提供该参数，在没有对应属性时，将触发AttributeError。|
|globals()|以字典类型返回当前位置的全部全局变量。|globals()||
|hasattr()|用于判断对象是否包含对应的属性。|hasattr(object, name)|object:对象，name: 字符串，属性名。|
|hash()|获取一个对象(字符串或数值)的哈希值|hash(object)|object: 对象|
|help()|查看函数或模块用途的详细说明|help(object)|object: 对象|
|hex()|用于将十进制整数转换成16进制，以字符串形式表示。|hex(x)|x: 10进制整数|
|id()|返回对象的唯一标识符，标识符是一个整数。CPython中id()函数用于获取对象的内存地址。|id([object])|object: 对象|
|input()|接受一个标准输入数据，返回为string类型|input([prompt])|prompt:提示信息|
|int()|用于将一个字符串或数字转换为整型。|int(x, base = 10)|返回整型数据。|x: 字符串或数字。base: 进制数，默认十进制。|
|isinstance()|判断一个对象是否是一个已知的类型，类似type()。|isinstance(object, classinfo)|object: 实例对象。classinfo: 可以是直接或间接类名、基本类型或者由他们组成的元组。|
|issubclass()|判断参数class是否是类型参数classinfo的子类。|issubclass(class, classinfo)|class: 类。classinfo: 类。|
|iter()|用来生成迭代器|iter(object[, sentine])|object: 支持迭代的集合对象。sentinel: 如果传递了第二个参数，则参数object必须是一个可调用的对象。iter创建了一个迭代器对象，每次调用这个迭代器对象_next_()方法时，都会调用object。|
|len()|返回对象(字符、列表、元组等)长度或项目个数。|len(s)|s: 对象|
|list()|将元组转换为列表|list(tup)|tup: 要转换为列表的元组|
|locals()|以字典类型返回当前位置的全部局部变量。|locals()||
|long()|将数字或字符串转换为一个长整型。|long(x, base=10)|x: 字符串或数字，base: 可选，进制数，默认十进制。|
|map()|根据提供的函数对指定序列做映射。第一个参数function以参数序列中的每一个元素调用function函数，返回包含每次function函数返回值的新列表。|map(function, iterable, ...)|function: 函数。iterable: 一个或多个序列|
|max()|返回给定参数的最大值，参数可以为序列。|max(x, y, z, ....)|x: 数值表达式。y: 数值表达式。y: 数值表达式|
|memoryview()|返回给定参数的内存查看对象。|memoryview(obj)|obj: 对象|
|min()|返回给定参数的最小值，参数可以为序列。|min(x, y, z, ....)|x: 数值表达式。y: 数值表达式。y: 数值表达式|
|next()|返回迭代器的下一个项目，要和生成迭代器iter()函数一起使用。|next(iterable[, default])|iterable: 可迭代对象，default: 可选，用于设置在没有下一个元素时返回该默认值，如果不设置，又没有下一个元素则会触发StopIteration异常|
|oct()|将一个整数转换成8进制字符串，3.x版本8进制以0o作为前缀表示。|oct(x)|x: 整数|
|open()|打开一个文件，创建一个file对象，相关的方法才可以调用它进行读写。|open(name[, mode[, buffering]])|name:一个包含了你要访问的文件名称的字符串值。mode: 只读，写入，追加等。buffering: 寄存区缓冲大小。|
|ord()|返回对应的ASCII数值，或者Unicode数值，如果所给的Unicode字符超出了你的Python定义范围，则会引发一个TypeError的异常。|ord(c)|c:字符|
|pow()|返回x^y(x的y次方)的值。|pow(x, y[, z])|x: 数值表达式。y: 数值表达式。z: 数值表达式。|
|print()|打印输出|print(*object, seq=' ',end='\n', file=sys.stdout, flush = False)|objects: 复数，表示可以一次输出多个对象。输出多个对象时，需要用,分隔。seq: 用来间隔多个对象，默认值是一个空格。end: 用来设计以什么结尾。默认值是换行符\n，我们可以换成其他字符串。file: 写入的文件对象。flush: 输出是否被缓存通常决定于file, 但如果flush关键字参数为true，流会被强制刷新。|
|property()|在新式类中返回属性值。|property([fget[, fset[, fdel[,doc]]]])|fget: 获取属性值的函数，fset: 设置属性值的函数。fdel: 删除属性值函数。doc: 属性描述信息。|
|range()|创建一个整数列表，一般用在for循环中|range(start, stop[,step])|start: 计数从start开始。默认是从0开始。stop: 计数到stop结束，但不包括stop。step: 步长，默认为1。|
|raw_input()|用来获取控制台的输入，将所有输入作为字符串看待，返回字符串类型。|raw_input([prompt])|prompt: 可选，字符串，可作为一个提示语。|
|reduce()|对参数序列中元素进行累积|reduce(function, iterable[, initializer])|function: 函数，有两个参数。iterable: 可迭代对象。initializer: 可选，初始参数。|
|reload()|用于重新载入之前载入的模块|reload(module)|module: 模块对象|
|repr()|将对象转化为供解释器读取的形式。返回一个对象的string格式。|repr(object|object: 对象|
|reverse()|反向列表中元素。|list.reverse()| |
|round()|返回浮点数x的四舍五入值|round(x [, n])|x: 数值表达式。n: 数值表达式，表示从小数点位数|
|set()|创建一个无序不重复元素集，可进行关系测试，删除重复数据，还可以计算交集、差集、并集等|set([iterable])|iterable: 可迭代对象。|
|setattr()|对应函数getattr(), 用于设置属性值，该属性不一定是存在的。|setattr(object, name, value)|object: 对象，name: 字符串，对象属性，value: 属性值|
|slice()|实现切片对象，用在切片操作函数里的参数传递。|slice(stop), slice(start, stop[, step])|start: 起始位置，stop: 结束位置。step: 间距|
|sorted()|对所有可迭代对象进行排序操作。sort是应用在list上的方法|sorted(iterable, cmp=None, key=None, reverse = False)|iterable: 可迭代对象，cmp: 比较的函数，key: 主要是用来进行比较的元素，reverse: 排序规则，reverse = True降序，reverse = False升序(默认)|
|staticmethod()|返回函数的静态方法|staticmethod(function)| |
|str()|将对象转化为适于人阅读的形式。|str(object)|object: 对象|
|sum()|对序列进行求和运算。|sum(iterable[, start])|iterable: 可迭代对象，如: 列表、元组、集合。start: 指定相加的参数，如果没有设置这个值，默认为0.|
|super()|调用父类(超类)的一个方法。解决多重继承问题。|super(type[, object-or-type])|type: 类。object-or-type: 类，一般是self。|
|tuple()|将列表转换为元组。|tuple(iterable)|iterable: 要转换为元组的可迭代序列。|
|type()|如果只有第一个参数则返回对象的类型，三个参数返回新的类型对象。|type(object), type(name, bases, dict)|name: 类的名称。bases: 基类的元组。dict: 字典|
|unichr()|和chr()功能基本一样，返回unicode字符。|unichr(i)|i: 可以是10进制也可以是16进制的形式的数字。|
|vars()|返回对象object的属性和属性值的字典对象。如果没有参数，就打印当前调用位置的属性和属性值|vars([object])|object: 对象。|
|xrange()|与range相同，所不同的是生成的不是一个数组，而是一个生成器。|xrange(stop), xrange(start, stop[, step])|start: 计数从start开始。默认是从0开始。stop: 计数到stop结束，但不包括stop。step: 步长，默认为1.|
|zip()|将可迭代的对象作为参数，将对象中对应的元素打包成一个个元组，然后返回由这些元组组成的列表，如果各个迭代器的元素个数不一致，则返回列表长度与最短的对象相同，利用*号操作符，可以将元组解压为列表。|zip([iterable, ...])|iterable: 一个或多个迭代器。|
|_import_()|用于动态加载类和函数。如果一个模块经常变化就可以使用_import_()来动态引入。|_import_(name,[,globals[,locals[,fromlist[,level]]]])|name: 模块名|
|exec|执行储存在字符串或文件中的Python语句，相比于eval，exec可以执行更复杂的Python代码。|exec obj|obj: 要执行的表达式。|

