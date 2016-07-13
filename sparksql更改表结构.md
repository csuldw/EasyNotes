## SparkSQL更改表结构

......
......

加载一个文件，得到如下的表结构：

```
scala> dfs.printSchema
root
 |-- name: string (nullable = true)
 |-- desc: string (nullable = true)
 |-- click: double (nullable = true)
 |-- view: double(nullable = true)
```

现在我需要将`click`和`view`转成的类型转成`Long`，操作如下:

首先需要定义一个函数，将表内的\`Double\`类型转为\`Long\`类型，函数如下：

```
val toLong = udf[Long, Double](_.toLong)
```

然后使用`withColumn`变换字段类型，代码如下：

```
val dfs2 = dfs.withColumn("click", toLong(dfs("click"))).withColumn("view", toLong(dfs("view")))
```

使用`printSchema`查看表结构：

```
scala> dfs2.printSchema
root
 |-- name : string (nullable = true)
 |-- desc : string (nullable = true)
 |-- click: long (nullable = true)
 |-- view: long (nullable = true)
```

