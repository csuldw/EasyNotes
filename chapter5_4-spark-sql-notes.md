# SparkSQL核心笔记


> 官方文档连接：[http://spark.apache.org/docs/latest/sql-programming-guide.html](http://spark.apache.org/docs/latest/sql-programming-guide.html)

2014年9月11日，发布Spark1.1.0。Spark1.1.0 变化较大是SparkSQL和MLlib。SparkSQL的前身是Shark。由于Shark自身的不完善，2014年6月1日Reynold Xin宣布：停止对Shark的开发。SparkSQL抛弃原有Shark的代码，汲取了Shark的一些优点，如内存列存储（In-Memory Columnar Storage）、Hive兼容性等，重新开发SparkSQL。

## SparkSQL优势

### 数据兼容
SparkSQL可以处理一切存储介质和各种格式的数据，目前来看不但兼容Hive，还可以从RDD、parquet文件、JSON文件中获取数据，未来版本甚至支持获取RDBMS数据以及cassandra等NOSQL数据；同时SparkSQL可以通过扩展功能来支持更多的数据类型，例如Kudo等。

### 计算能力
由于SparkSQL基于Spark的高效计算引擎，导致其对于数据仓库的计算能力具有级大规模的提升(尤其在2.0版本钨丝计划成熟以后)，提升包括两方面：

1. 计算速度： Spark的强大计算能力为保证，提升计算速度。
2. 计算复杂度：由于SparkSQL可以直接操作DataFrame，从而使数据仓库的数据可以直接使用复杂算法库中的算法(例如机器学习，图计算等)，从而可以进行复杂升读的数据价值的挖掘。



## DataFrame简介

### 理解

Spark的DataFrame可以简单的理解为一个分布式的二维表，这样也就意味着他的每一列都带有名称和类型，也就意味着SparkSQL在基于每一列数据的元数据进行更加细粒度的分析，而不是如同以往分析RDD的时候那种粗粒度的分析。也就意味着这样SparkSQL基于DataFrame可以进行更加高效的性能优化。




