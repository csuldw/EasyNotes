# Python小技巧shape、extend、append、tile、numpy.sum



本文主要记录在学习过程中碰到的易混淆的Python语法。

### shape函数
python中的shape有两个，一个是numpy中的shape，一个是array的shape，本质上是一样的。

```
import numpy as np
y=[1,2,3,1,2,2,1,2,1,2]  # list
y_arr = array(y)         #array
y_mat = np.mat(y_arr)  #matrix
y_arr.shape[0]   #output:10L
y_mat.shape[0]   #output:1L
y_mat.shape[1]   #output:10L
np.shape(y_arr)  #output :(1L, 10L)
```

### extend和append

对于python中的list，假设有a=[1,2,3], b=[4,5,4]，如果使用a.append(b),结果为：

	[1,2,3,[4,5,4]]

如果使用a.extend(b),结果为：

	[1,2,3,4,5,4]


### numpy中的tile函数

看下面几个列子就懂了

```
import numpy as np
x = np.array([1,2,3])
x1 = np.tile(x,1)   # array([1,2,3])
x2 = np.tile(x,2)   # array([1,2,3,1,2,3])
x3 = np.tile(x,(1,2))   # array([1,2,3,1,2,3])
x4 = np.tile(x,(2,2))   # array([1,2,3,1,2,3],[1,2,3,1,2,3])
x5 = np.tile(x,(2,1))   # array([1,2,3],[1,2,3])
```

其实就相当于在tile的第二个参数中指定维数，将第一个变量的值整块填写到里面即可。

### numpy中的sum函数axis=1

对于sum看起来挺简单的样子，但是在给sum函数中加入参数。`sum(a, axis=0)`或者是`.sum(axis=1) `就有点不解了。在我实验以后发现,我们平时用的`sum`应该是默认的`axis=0`就是普通的相加。当加入`axis=1`以后就是将一个矩阵的每一行向量相加：

例如：

```
import numpy as np
x = np.array([[0,1,2],[2,1,3]])
np.sum(x, axis=1)
```

返回的结果就是：array（[3,6]）

`np.sum(x, axis=1)`等价于`x.sum(axis=1)`，注意：python中list没有sum属性【'list' object has no attribute 'sum'】。


如果使用的是`x.sum(axis=0)`，则返回的结果是：array([2, 2, 5])

它是对x的每一进行列求和运算。


### dict中的get()方法

get()方法返回给定键的值。如果键不可用，则返回默认值None。

语法
以下是get()方法的语法：

```
dict.get(key, default=None)
```

参数:

- key: 这是要搜索在字典中的键。
- default: 这是要返回键不存在的的情况下默认值。
- 返回值: 该方法返回一个给定键的值。如果键不可用，则返回默认值为None。


### operator的itemgetter()

operator模块提供的itemgetter函数用于获取对象的哪些维的数据，参数为一些序号（即需要获取的数据在对象中的序号）.

例如：

```
a = [1,2,3] 
b=operator.itemgetter(1)
b(a)   #输出2
```
要注意，operator.itemgetter函数获取的不是值，而是定义了一个函数，通过该函数作用到对象上才能获取值。

Python内置的排序函数sorted可以对list或者iterator进行排序

sorted(iterable[, cmp[, key[, reverse]]])

第一个参数是一个iterable，返回值是一个对iterable中元素进行排序后的列表(list)。

可选的参数有三个，cmp、key和reverse。
1)cmp指定一个定制的比较函数，这个函数接收两个参数（iterable的元素），如果第一个参数小于第二个参数，返回一个负数；如果第一个参数等于第二个参数，返回零；如果第一个参数大于第二个参数，返回一个正数。默认值为None。
2)key指定一个接收一个参数的函数，这个函数用于从每个元素中提取一个用于比较的关键字。默认值为None。
3)reverse是一个布尔值。如果设置为True，列表元素将被倒序排列。
通常来说，key和reverse比一个等价的cmp函数处理速度要快。这是因为对于每个列表元素，cmp都会被调用多次，而key和reverse只被调用一次。


比如下面一个例子：

```
b = {0:1, 1:5}
sorted(b.iteritems(), key=operator.itemgetter(1), reverse=True)   #表示按照第二维进行降序
```

输出：[(1, 5), (0, 1)]


### 步进切片

使用步进切片可以轻松实现sequence（list, tuple, string）反转

```
>>>name = 'AlphaGO'
>>>name[::-1]
Out[121]: 'OGahplA'

>>>name = ["A", "is",  "C"]
>>>name[::-1]
Out[125]: ['C', 'is', 'A']
```


### 均值列表

比如有三个列表，列表元素均为数值型，三个列表的长度都一样，现在我想要求这三个列表的均值，即求一个均值列表，对应元素为上述三个列表对应元素的均值。

<!--more-->


代码实现如下：

```
def meanMethod(one,two,three):
    comb = zip(one,two,three)
    return [float(i+j+k)/3 for i,j,k in comb]
```

第二行使用的是zip函数，先将三个列表合并起来，zip函数返回的是一个列表，但里面的元素是一个元组。

第三行是列表推导式，计算comb每个元组的均值。
