# Spark算子-flatMap


spark 1.6.1 api给出的是如下说明：

```
def flatMap[U](f: (T) ⇒ TraversableOnce[U])(implicit arg0: ClassTag[U]): RDD[U]
//Return a new RDD by first applying a function to all elements of this RDD, and then flattening the results.
```

## 理解

flatMap执行的是两个操作----“**先映射后扁平化**”：

Process 1：对每条element进行指定操作，然后为每条输入返回一个对象；
Process 2：将所有对象合并为一个对象；

## 实例-数据扁平化

使用`flatMap`将`RDD[Array[String]]`转换成`RDD[String]`的结果。

在spark-shell [`spark-shell --driver-memory 10G --executor-memory 10G --executor-cores 5 --num-executors 3` ]执行下列代码

```
val initRDD ：RDD[Array[String]] = sc.makeRDD(Array(Array("1","2"), Array("3","4")), 3)
val rdd2 = initRDD.flatMap(x => x.toList)
rdd2.collect
```

**输出结果**

```
output: Array[String] = Array(1, 2, 3, 4)
```