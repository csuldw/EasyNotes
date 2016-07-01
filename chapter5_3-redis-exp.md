# Spark批量读取Redis数据-Pipeline

最近在处理数据时，需要将原始数据与Redis的数据进行join，在读取Redis的过程中，碰到了一些问题，顺便做个笔记，希望对其他同学也有所帮助。实验过程中，当数据量还是十万级别的时候，逐个读取Redis并无压力；但当数据量达到千万级别时，问题就油然而生了，即使是使用Spark的mapPartitions也无法解决。因此，就考虑使用Redis的pipeline了（如果你有更好的方法，还请不吝赐教）。PS：本文主要针对的是Scala语言，因为目前在网上还没有看到Scala版本的Redis pipeline，希望此文能给初学者提供一个参考。

文章会先介绍如何使用Scala逐个去读取Redis数据，然后再介绍pipeline的使用。

## 方法一、逐行读取Redis数据

在本文，主要使用的是`redis.clients.jedis.Jedis`库，如果你是使用sbt来运行spark，可以在build.sbt中做如下配置：

```
name := "sparkRedisExp"

version := "1.0.0"

scalaVersion := "2.10.4"

libraryDependencies += "org.apache.spark" %% "spark-core" % "1.3.1"

libraryDependencies += "redis.clients" % "jedis" % "2.6.2"

resolvers += "Akka Respository" at "http://repo.akka.io/releases/"
```

相应的jedis库可以到Github中下载 jedis-2.6.2.jar：[https://github.com/csuldw/WorkUtils/tree/master/Spark/deps](https://github.com/csuldw/WorkUtils/tree/master/Spark/deps)。下面请看详细内容。

### 导入Redis库

首先导入redis库，这里使用redis.clients.jedis.Jedis库。

```
import redis.clients.jedis.Jedis
```

### 连接Redis

然后连接Redis，主要设置`redisHost`、`redisPort`，如果有密码，需要进行密码验证。

```
val redisHost = "localhost"
val redisPort = 8080
val redisClient = new Jedis(redisHost, redisPort)
redisClient.auth(redisPassword)
```

### 读取Redis数据

接下来，就可以直接使用get获取redis数据

```
val keys = Array("key1", "key2", "key3", "key4")
for(key <- keys){
  println(redisClient.get(key))
}
```

上述方法并没有使用Redis的pipeline，当数据较少的时候，可以用来使用。下面介绍如何使用pipeline来批量读取Redis数据。


## 方法二、使用Redis pipeline批量读取Redis数据

相对于第一种方法，这里需要额外引入两个bao，`redis.clients.jedis.Pipeline`和`redis.clients.jedis.Response`。

### 导入相关库

```
import redis.clients.jedis.Jedis
import redis.clients.jedis.Pipeline
import redis.clients.jedis.Response
```

### 连接Redis

此操作与上面的一样，如下：

```
val redisHost = "localhost"
val redisPort = 8080
val redisClient = new Jedis(redisHost, redisPort)
redisClient.auth(redisPassword)
```
### 使用pipeline读取数据之一（简化版）

先给出代码，下面再做解释。

```Scala
var tempRedisRes = Map[String, Response[String]]()
val keys = Array("key1", "key2", "key3", "key4")
val pp = redisClient.pipelined()
for(key <- keys){
  tempRedisRes ++= Map(key -> pp.get(key)) 
}
pp.sync()
```


因为`redis.clients.jedis.Jedis`的`pipelined`下的`get`方法获取的是一个`Response[String]`类型的返回值，所以上面定义了一个临时变量`Map[String, Response[String]]`类型的`tempRedisRes`，key是String类型，value是Response[String]类型，用于保存`pp.get(key)`的返回值。当for循环执行完之后，使用sync同步即可。这样便实现了Redis的Pipeline功能。


### 使用pipeline读取数据之二（强化版）

为了防止连接Redis时的意外失败，我们需要设置一个尝试次数，确保数据一定程度上的正确性。因此，在上面的代码外面增加一层连接逻辑，如下：

```
var tempRedisRes = Map[String, Response[String]]()
val keys = Array("key1", "key2", "key3", "key4")
var tryTimes = 2
var flag = false
while(tryTimes > 0 && !flag) {
  try{
    val pp = redisClient.pipelined()
    for(key <- keys){
      tempRedisRes ++= Map(key -> pp.get(key))
    }
    pp.sync()
    flag = true
  }catch {
    case e: Exception => {
      flag = false
      println("Redis-Timeout" + e)
      tryTimes = tryTimes - 1
    }
  }finally{
    redisClient.disconnect()
  }
}
```

再次说明：`pp.get()`得到的是一个`Response[String]`的结果，详细内容请查看 [redis-clients-jedis-Pipeline](http://tool.oschina.net/uploads/apidocs/jedis-2.1.0/redis/clients/jedis/Pipeline.html).

Ok，本文内容到此结束，更多精彩文章，请进 [http://www.csuldw.com](http://www.csuldw.com).
