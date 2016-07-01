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


### 与RDD的区别

DataFrame的数据特点是其包含了每个Record的Metadata信息，让你可以在优化时基于列内部进行优化(例如一共30列，你只需要其中10列，你就可以只获取其中10列的信息)。RDD的数据特点有点是框架只知道每个Record是个person,但是无法洞悉内部是什么，这样也就导致了框架无法洞悉Record内部的细节，也就在优化的时候闲置了SparkSQL的性能提升。


## 入门指南


### 一、引入需要的库

```
import org.apache.spark.SparkContext

import org.apache.spark.SparkConf
import org.apache.spark.SparkContext._
import org.apache.spark.rdd.RDD
import org.apache.commons.configuration.{ PropertiesConfiguration => HierConf }
import org.apache.spark.sql.types._

import org.apache.spark.sql._
import org.apache.spark.sql.functions._
```

### 二、初始化SparkSQL

```
val sc: SparkContext // An existing SparkContext.
val spSQL = new org.apache.spark.sql.SQLContext(sc)

// this is used to implicitly convert an RDD to a DataFrame.
import spSQL.implicits._
```

### 三、读取HDFS文件


```
inputFile="hdfs://master:9000/home/liudiwei/log.data"
val df = spSQL.jsonFile(inputFile)    // Create the DataFrame
```

注意：`inputFile`是HDFS路径，文件内容以JSON格式存储。



## DataFrame基本操作

下面引用api里面的实例

```
df.show()
// age  name
// null Michael
// 30   Andy
// 19   Justin

// Print the schema in a tree format
df.printSchema()
// root
// |-- age: long (nullable = true)
// |-- name: string (nullable = true)

// Select only the "name" column
df.select("name").show()
// name
// Michael
// Andy
// Justin

// Select everybody, but increment the age by 1
df.select(df("name"), df("age") + 1).show()
// name    (age + 1)
// Michael null
// Andy    31
// Justin  20

// Select people older than 21
df.filter(df("age") > 21).show()
// age name
// 30  Andy

// Count people by age
df.groupBy("age").count().show()
// age  count
// null 1
// 19   1
// 30   1
```

> 其中，select、groupBy +　count 是平时用到比较多的，尤其是groupBy结合count来统计数据，比较方便。show主要用于spark-shell里进行调试输出。



## 使用SQL语言操作数据

假设表中有一个字段是uid，表示的是用户的MD5标识，现在需要通过spark sql代码将用户取出来，并去重。代码如下：


方法一：spark SQL代码

```
//get user MD5 list
df.registerTempTable("basisData")
def getUser(spSQL: SQLContext) : RDD[String] = {
  spSQL.sql(s"""SELECT DISTINCT uid from basisData""").map{ case Row(uid) => uid.toString }
}
```

方法二、内嵌函数select

```
df.select("uid").map(x => x.toString)
```


## 实例

加入原始数据是下面这种，uid表示用户，obj表示某个对象，action表示对这个对象的操作(只有click和view两类)。

```
uid obj action
u1  s1  view
u1  s1  view
u1  s1  click
u2  s1  view
u1  s2  view
u2  s2  view
u2  s2  click 
```


现在我的目的是统计每个用户对各个对象的click和view的总数各是多少，需要得到结果如下：

```
uid obj click view
u1  s1   1      2
u1  s2   0      1
u2  s1   0      1
u2  s2   1      1
```

下面使用sparkSQL来解决问题。

加入我现在的数据以json格式存储在data.file中，现在先加载数据：

```
import org.apache.spark.SparkContext

import org.apache.spark.SparkConf
import org.apache.spark.SparkContext._
import org.apache.spark.rdd.RDD
import org.apache.commons.configuration.{ PropertiesConfiguration => HierConf }
import org.apache.spark.sql.types._

import org.apache.spark.sql._
import org.apache.spark.sql.functions._

val spSQL = new org.apache.spark.sql.SQLContext(sc)
import spSQL.implicits._

val userDataDF = spSQL.jsonFile(inputFile)
```

然后使用`groupBy`和`count`进行聚合:

```
userDataDF.groupBy("uid", "obj", "action").count
```

此时得到的结果是：

```
uid obj action count
u1  s1  click   1
u1  s1  view    2
u1  s2  view    1
u2  s1  view    1
u2  s2  click   1
u2  s2  view    1
```


下面编写一个通用函数

```
/* 功能:获取用户行为数据
 * dagaDF: 基础数据过滤后的数据
 * regName：设置sparkSQL的registerTempTable名字 
 * selectedCols：选中表中的列，多个以逗号分隔，结尾无逗号
 * 返回：包含uid, typeCol, click, view的DataFrame数据
 */
def getCmbActionList(spSQL: SQLContext, dataDF : DataFrame, regName : String, selectedCols : String) : DataFrame = {
  dataDF.registerTempTable(regName)
  val sps = spSQL.sql(s"""SELECT $selectedCols, 
    sum(CASE WHEN action="click" THEN count ELSE 0 END) click, 
    sum(CASE WHEN action="view" THEN count ELSE 0 END) view 
    FROM $regName
    GROUP BY $selectedCols""")
  sps
}
```

调用该函数之后，便可得到需要的结果

```
val actionRes = getCmbActionList(spSQL, userDataDF, "userDataDFs", "uid,obj")
```





































