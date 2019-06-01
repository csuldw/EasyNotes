# Spark算子-combineByKey

combineByKey属于Key-Value型算子，做的是聚集操作，这种变换不会触发作业的提交，主要有三个参数，分别是:

* combiner function : 组合器函数，用于将RDD\[K,V\]中的V转换成一个新的值C1；
* mergeValue function ：合并值函数，将一个C1类型值和一个V类型值合并成一个C2类型，输入参数为\(C1,V\)，输出为新的C2
* mergeCombiners function ：合并组合器函数，用于将两个C2类型值合并成一个C3类型，输入参数为\(C2,C2\)，输出为新的C3

下面通过一个实例来理解。

## 实例

先来看看代码

```text
outInfo.combineByKey(
  (v) => (1, v),
  (acc: (Int, String), v) => (acc._1 + 1, acc._2),
  (acc1: (Int, String), acc2: (Int, String)) => (acc1._1 + acc2._1, acc2._2)
).sortBy(_._2, false).map{
  case (key, (key1, value)) => Array(key, key1.toString, value).mkString("\t")
}.saveAsTextFile("/out")
```

outInfo 是一个RDD，数据类型\(K: String, V: String\)，测试数据如下

```text
("hello", "world")
("hello", "ketty")
("hello", "Tom")
("Sam", "love")
("Sam", "sorry")
("Tom", "big")
("Tom", "shy")
```

现在，我的目的是按key值统计数据并对key去重，然后将每个key的最后一次出现的value作为value的第二个元素，即（key，count，value），可以通过combimeByKey将上列数据转换成下列结果：

```text
"hello"\t3\t"Tom"
"Sam"\t2\t"soryy"
"Tom"\t2\t"shy"
```

详细解释：首先定义combiner function表达式`(v) => (1, v)`,可以将一个\("hello", "world"\)转换成 \("hello", \(1, "world"\)\); 然后定义mergeValue function表达式 `(acc: (Int, String), v) => (acc._1 + 1, acc._2)`, 可以将\(\("hello", \(1, "world"\)\)、\("hello", "ketty"\) 转换成\("hello", \(2, "ketty"\)\); 然后定义mergeCombiners function表达式`(acc1: (Int, String), acc2: (Int, String)) => (acc1._1 + acc2._1, acc2._2)`可以讲\("hello", \(2, "ketty"\)\)、\("hello", \(1, "Tom"\)\)转换成 \("hello", \(3, "Tom"\)\). 最后对count进行排序，并以 "hello"\t3\t"Tom" 格式化输出。

