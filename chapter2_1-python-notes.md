# Python笔记

### 判断字符串是否属于浮点类型

先使用eval函数，然后使用type判断。关于`eval()`函数，官方解释是：将字符串str当成有效的表达式来求值并返回计算结果。而`type()`可以得到一个对象的类型。

```
type(eval("123")) == int
type(eval("123.23")) == float
```

### 判断实例是否是某个对象

使用`isinstance(object, class-ro-type-or-tuple)`，可以判断实例是否是这个类或者object是变量

```
In [1]: isinstance('1.11', int)
Out[1]: False
```

### 字符串的各种判断方法

```
s="123455"
s.isalnum()  #所有字符都是数字或者字母
s.isalpha()  #所有字符都是字母
s.isdigit()  #所有字符都是数字
s.islower()  #所有字符都是小写
s.isupper()  #所有字符都是大写
s.istitle()  #所有单词都是首字母大写，像标题
s.isspace()  #所有字符都是空白字符、\t、\n、\r
```

