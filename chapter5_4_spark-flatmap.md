# Spark算子-flatMap


使用`flatMap`将
```
val initRDD ：RDD[Array[String]] = sc.makeRDD(Array(Array("1","2"), Array("3","4")), 3)
val rdd2 = initRDD.flatMap(x => x.toList)
rdd2.collect
```
输出结果

```
output: Array[String] = Array(1, 2, 3, 4)
```