# SparkSQL核心笔记


> 官方文档连接：[http://spark.apache.org/docs/latest/sql-programming-guide.html](http://spark.apache.org/docs/latest/sql-programming-guide.html)


## SparkSQL发展历史

- 2014年9月11日，发布Spark1.1.0。Spark1.1.0变化较大是SparkSQL和MLlib。
  - SparkSQL的前身是Shark。由于Shark自身的不完善，2014年6月1日Reynold Xin宣布：停止对Shark的开发。SparkSQL抛弃原有Shark的代码，汲取了Shark的一些优点，如内存列存储（In-Memory Columnar Storage）、Hive兼容性等，重新开发SparkSQL。

