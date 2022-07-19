---
title: NumPy数据类型 & 数组
date: 2022-04-29 19:30:00
categories:
 - [机器学习,python,numpy]
tags: 
 - numpy
 - 机器学习
 - 数学算法
---

# NumPy数据类型

numpy支持的数据类型比Python内置的类型要多很多，基本上可以与C语言的数据类型对应上，其中部分类型对应为Python内置的类型。
|名称|描述|
|:--|:----|
|bool_|布尔型数据类型(True或者False)|
|int_|默认的整数类型(类似C语言中的long，int32或int64)|
|intc|与C的int类型一样，一般是int32或int64|
|intp|用于索引的整数类型(类似C的ssize_t, 一般情况下仍然是int32或int64)|
|int8|字节(-128 to 127)|
|int16|整数(-32768 to 32767)|
|int32|整数(-2147483648 to 214748647)|
|int64|整数(-9223372036854775808 to 9223372036854775807)|
|uint8|无符号整数(0 to 255)|
|uint32|无符号整数(0 to 65536)|
|uint64|无符号整数(0 to 18446744073709551615)|
|float_|float64类型的简写|
|float16|半精度浮点数，包括：1个符号位，5个指数位，10个尾数位|
|float32|单精度浮点数，包括：1个符号位，8个指数位，23个尾数位|
|float64|双精度浮点数，包括：1个符号位，11个指数位，52个尾数位|
|complex_|complex128类型的简写，即128位复数|
|complex64|复数，表示双32位浮点数(实数部分和虚数部分)|
|complex128|复数，表示双64位浮点数(实数部分和虚数部分)|

numpy的数值类型实际上是dtype对象的实例，并对应唯一的字符。

# 数据类型对象(dtype)

数据类型对象(numpy.dtype类的实例)用来描述与数组对应的内存区域是如何使用，它描述了数据的以下几个方面：
- 数据的类型(整数，浮点数或者Python对象)
- 数据的大小(例如，整数使用多少个字节存储)
- 数据的字节顺序(小端法或大端法)
- 在结构化类型的情况下，字段的名称、每个字段的数据类型和每个字段索取的内存块的部分
- 如果数据类型是子数组，那么它的形状和数据类型是什么

字节顺序是通过对数据类型预先设定`<`或`>`来决定的。`<`意味着小端法(最小值存储在最小的地址，即低位组放在最前面)。

dtype对象是使用以下语法构造的：
:::default no-icon
numpy.dtype(obejct, align, copy)
:::

object - 要转换的数据类型对象
align - 如果为true，填充字段使其类似C的结构体
copy - 复制dtype对象，如果为false，则是对内置数据类型对象的引用

实例:
```python
import numpy as np
# 使用标量类型
dt = np.dtype(np.int32)
print(dt)
```
:::default no-icon
int32
:::

```python
import numpy as np
# int8, int16, int32, int64 四种数据类型可以使用字符串'i1','i2','i4','i8'代替
dt = np.dtype('i4')
print(dt)
```

:::default no-icon
int32
:::

```python
import numpy as np
# int8, int16, int32, int64 四种数据类型可以使用字符串'i1', 'i2', 'i4', 'i8'
dt = np.dtype('i4')
print(dt)
```
:::default no-icon
int32
:::

```python
# 首先创建结构化数据类型
import numpy as np
dt = np.dtype([('age', np.int8)])
```
:::default no-icon
[('age', 'i1')]
:::

```python
# 将数据类型应用于ndarray对象
import numpy as np
dt = np.dtype([('age', np.int8)])
a = np.array([(10,),(20,),(30,)], dtype = dt)
print(a)
```
:::default no-icon
[(10,), (20,) (30,)]
:::

每个内建类型都有一个唯一定义它的字符代码。
|字符|对应类型|
|:---|:------|
|b|布尔型|
|i|(有符号)整形|
|u|无符号整型integer|
|f|浮点型|
|c|复数浮点型|
|m|timedelta(时间间隔)|
|M|datetime(日期时间)|
|O|(Python)对象|
|S, a|(byte-)字符串|
|U|Unicode|
|V|原始数据(void)|

# NumPy 数组属性

NumPy数组的维数成为秩(rank)，秩就是轴的数量，即数组的维度，一维数组的秩为1，二维数组的秩为2，以此类推。在NumPy中，每一个线性的数组称为是一个轴(axis)，也就是维度(dimensions)。比如说，二维数组相当于是两个一维数组，其中第一个一维数组中每个元素又是一个一维数组。所以一维数组就是NumPy中的轴(axis)，第一个轴相当于是低层数组，第二个轴是低层数组里的数组。而轴的数量--秩，就是数组的维数。
很多时候可以声明axis。axis=0，表示沿着第0轴进行操作，即对每一列进行操作；axis=1，表示沿着第一轴进行操作，即对每一行进行操作。
NumPy的数组中比较重要ndarray对象属性有:
|属性|说明|
|:---|:---|
|ndarray.ndim|秩，即轴的数量或维度的数量|
|ndarray.size|数组元素的总个数，相当于.shape中n*m的值|
|ndarray.dtype|ndarray对象的元素类型|
|ndarray.itemsize|ndarray对象中每个元素的大小，以字节为单位|
|ndarray.flags|ndarray对象的内存信息|
|ndarray.real|ndarray元素的实部|
|ndarray.imag|ndarray元素的虚部|
|ndarray.data|包含实际数组元素的缓冲区，由于一般通过数组的索引获取元素，所以通常不需要使用这个属性。|

## ndarray.ndim

ndarray.ndim用于返回数组的维数，等于秩
```python
import numpy as np

a = np.arange(24)
print(a.ndim)
# 现在调整其大小
b = a.reshape(2,4,4)
print (b.ndim)
```
:::default no-icon
1
3
:::

## ndarray.shape

ndarray.shape表示数组的维度，返回一个元组，这个元组的长度就是维度的数目，即ndim属性(秩)。比如，一个二维数组，其维度表示“行数”和“列数”
ndarray.shape也可以用于调整数组大小。

```python
import numpy as np

a = np.array([[1,2,3],[4,5,6]])
print (a.shape)
```
输出结果:
:::default no-icon
(2,3)
:::

## ndarray.itemsize

ndarray.itemsize以字节的形式返回数组中每一个元素的大小。
一个元素类型为float64的数组itemsize属性值为8(float64占用64个bits, 每个字节长度为8, 所以64/8，占用8个字节)，又如，一个元素类型为complex32的数组item属性为4(32/8)
实例:
```python
import numpy as np

# 数组的dtype为int8 (一个字节)
x = np.array([1,2,3,4,5], dtype = np.int8)
print(x.itemsize)

# 数组的dtype现在为float64(八个字节)
y = np.array([1,2,3,4,5], dtype = np.float64)
print(y.itemsize)
```
:::default no-icon
1
8
:::

## ndarray.flags

ndarray.flags返回ndarray对象的内存信息，包含以下属性:
|属性|描述|
|:---|:---|
|C_CONTIGUOUS(C)|数据是在一个单一的C风格的连续段中|
|F_CONTIGUOUS(F)|数据是在一个单一的Fortran风格的连续段中|
|OWNDATA(O)|数组拥有它所使用的内存或从另一个对象中借用它|
|WRITEABLE(W)|数据区域可以被写入，将该值设置为False，则数据为只读|
|ALIGNED(A)|数据和所有元素都适当地对齐到硬件上|
|UPDATEIFCOPY(U)|这个数组是其他数组的一个副本，当这个数组被释放时，原数组的内容将被更新。|

实例：
```python 
import numpy as np
x = np.array([1,2,3,4,5])
print (x.flags)
```
:::default no-icon
  C_CONTIGUOUS : True
  F_CONTIGUOUS : True
  OWNDATA : True
  WRITEABLE : True
  ALIGNED : True
  WRITEBACKIFCOPY : False
  UPDATEIFCOPY : False
:::

# NumPy 创建数组

ndarray数组除了可以使用底层ndarray构造器来创建外，也可以通过以下几种方式来创建。

## numpy.empty

numpy.empty方法用来创建一个指定形状(shape)、数据类型(dtype)且未初始化的数组：
```python
numpy.empty(shape, dtype = float, order = 'C')
```

参数说明:
|参数|描述|
|:--|:---|
|shape|数组形状|
|dtype|数据类型，可选|
|order|有"C"和"F"两个选项，分别代表，行优先和列优先，在计算机内存中的存储元素的顺序。|

```python
import numpy as np
x = np.empty([3,2], dtype = int)
print (x)
```
:::default no-icon
[[ 6917529027641081856  5764616291768666155]
 [ 6917529027641081859 -5764598754299804209]
 [          4497473538      844429428932120]]
:::
**注意**-数组元素为随机值，因为他们未初始化。

## numpy.zeros

创建指定大小的数组，数组元素以0来填充:
```python
numpy.zeros(shape, dtype = float, order = 'C')
```
参数说明:
|参数|描述|
|:---|:---|
|shape|数组形状|
|dtype|数据类型，可选|
|order|'C'用于C的行数组，或者'F'用于FORTRAN的列数组|

## numpy.ones

创建指定形状的数组，数组元素以1来填充:
```python
numpy.ones(shape, dtype = None, order = 'C')
```
|参数|描述|
|:---|:---|
|shape|数组形状|
|dtype|数据类型，可选|
|order|'C'用于C的行数组，或者'F'用于FORTRAN的列数组|

# NumPy从已有的数组创建数组

## numpy.asarray

numpy.asarray类似numpy.array, 但numpy.asarray参数只有三个，比numpy.array少两个。
```python 
numpy.asarray(a, dtype = None, order = None)
```

|参数|描述|
|:---|:---|
|a|任意形式的输入参数，可以是列表、列表的元组、元组、元组的元组、元组的列表、多维数组|
|dtype|数据类型，可选|
|order|'C'用于C的行数组，或者'F'用于FORTRAN的列数组|

实例：
```python
import numpy as np

x = [1,2,3]
a = np.asarray(x)
print(a)

x = [(1,2,3),(4,5)]
a = np.asarray(x)
print(a)
```

结果: [1 2 3] [(1,2,3)(4,5)]

## numpy.frombuffer

numpy.frombuffer用于实现动态数组。
numpy.frombuffer接受buffer输入参数，以流的形式读入转化成ndarray对象
```python
numpy.frombuffer(buffer, dtype = float, count = -1, offset = 0)
```
|参数|描述|
|:---|:---|
|buffer|可以是任意对象，会以流的形式读入。|
|dtype|返回数组的数据类型，可选|
|count|读取的数据数量，默认为-1，读取所有数据|
|offset|读取的起始位置，默认为0|

```python 
import numpy as np

s = b'Hello World'
a = np.frombuffer(s, dtype = 'S1')
print (a)
```
输出结果:
:::default no-icon
[b'H' b'e' b'l' b'l' b'o' b' ' b'W' b'o' b'r' b'l' b'd']
:::

## numpy.fromiter

numpy.fromiter方法从可迭代对象中建立ndarray对象，返回一维数组。
```python
numpy.fromiter(iterable, dtype, count = -1)
```
|参数|描述|
|:---|:---|
|iterable|可迭代对象|
|dtype|返回数组的数据类型|
|count|读取的数据数量，默认为-1，读取所有数据|

```python
import numpy as np

# 使用range函数创建列表对象
list = range(5)
it = iter(list)

# 使用迭代器创建 ndarray
x = np.fromiter(it, dtype = float)
print(x)
```
输出结果：
:::default no-icon
[0. 1. 2. 3. 4.]

# NumPy 从数值范围创建数组

## numpy.arange

numpy包中的使用arange函数创建数值范围并返回adarray对象，函数格式如下:
```python
numpy.arange(start, stop, step, dtype)
```
根据start与stop指定的范围以及step设定的步长，生成一个ndarray。
参数说明:
|参数|描述|
|:---|:--|
|start|起始值, 默认为0|
|stop|终止值(不包含)|
|step|步长, 默认为1|
|dtype|返回ndarray的数据类型，如果没有提供，则会使用输入数据的类型|

```python
import numpy as np

x = np.arange(5)
print(x)
```
:::default no-icon
[0. 1. 2. 3. 4.]
:::

设置了起始值、终止值及步长:
```python
import numpy as np
x = np.arange(10,20,2)
print (x)
```
输出结果:
:::default no-icon
[10 12 14 16 18]
:::

## numpy.linspace

numpy.linspace函数用于创建一个一维数组，数组是一个等差数列构成
```python
np.linspace(start, stop, num = 50, endpoint = True, retstep = False, dtype = None)
```

参数说明:
|说明|描述|
|:---|:---|
|start|序列的起始值|
|stop|序列的终止值，如果endpoint为true，该值包含于数列中|
|num|要生成的等步长的样本数量，默认为50|
|endpoint|该值为true时，数列中包含stop值，反之不包含，默认是True|
|retstep|如果为True时，生成的数组中会显示间距，反之不显示|
|dtype|ndarray的数据类型|

```python
import numpy as np
a = np.linspace(1, 10, 10)
print(a)
```

输出结果:
:::default no-icon
[1. 2. 3. 4. 5. 6. 7. 8. 9. 10.]
:::

```python
import numpy as np
a = np.linspace(1, 10, 10)
print (a)

a = np.linspace(1, 1, 10)
print (a)
```
输出结果:
:::default no-icon
[1. 2. 3. 4. 5. 6. 7. 8. 9. 10.]
[1. 1. 1. 1. 1. 1. 1. 1. 1. 1.]
:::
将endpoint设为false，不包含终止值

```python
import numpy as np

a = np.linspace(10, 20, 5, endpoint = False)
print (a)
```
输出结果:
:::default no-icon
[10. 12. 14. 16. 18.]
:::

如果将endpoint设为true，则会包含20.

```python
import numpy as np
a = np.linspace(1,10,10,retstep = True)

print (a)
# 拓展例子
b = np.linspace(1,10,10).reshape([10,1])
print(b)
```
输出结果为:
:::default no-icon
(array([1., 2., 3., 4., 5., 6., 7., 8., 9., 10.]), 1.0)
[[1.]
 [2.]
 [3.]
 [4.]
 [5.]
 [6.]
 [7.]
 [8.]
 [9.]
 [10.]]
:::

## numpy.logspace

numpy.logspace 函数用于创建一个于等比数列。
```python
np.logspace(start, stop, num = 50, endpoint = True, base = 10.0, dtype = None)
```

base参数意思是取对数的时候log的下标
|参数|描述|
|:---|:--|
|start|序列的起始值为: base**start|
|stop|序列的终止值: base**stop。如果endpoint为true, 该值包含于数列中。|
|num|要生成的等步长的样本数量，默认为50|
|endpoint|该值为true时，数列中包含stop值，反之不包含，默认是True|
|base|对数log的底数。|
|dtype|ndarray的数据类型|

实例:
```python
import numpy as np
# 默认底数是 10
a = np.logspace(1.0, 2.0, num = 10)
print (a)

a = np.logspace(0, 9, 10, base = 2)
pritn (a)
```

输出结果为:
:::default no-icon
[ 10.           12.91549665     16.68100537      21.5443469  27.82559402      
  35.93813664   46.41588834     59.94842503      77.42636827    100.    ]

[  1.   2.   4.   8.  16.  32.  64. 128. 256. 512.]
:::




