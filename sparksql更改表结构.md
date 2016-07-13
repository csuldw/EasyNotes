## SparkSQL更改表结构



定义一个函数，将表内的\`Double\`类型转为\`Long\`类型


```
val toLong = udf[Long, Double](_.toLong)
```


