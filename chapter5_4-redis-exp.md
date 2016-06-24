# Redis读取

## 方法一、逐行读取Redis数据

### 导入Redis库
首先导入redis库，这里使用redis.clients.jedis.Jedis库。
```
import redis.clients.jedis.Jedis
```

### 连接Redis
主要设置redisHost、redisPort，如果有密码，需要进行密码验证。

```
val redisHost = "localhost"
val redisPort = 8080
val redisClient = new Jedis(redisHost, redisPort)
redisClient.auth(redisPassword)
```

### 读取Redis数据
可以直接使用get获取redis数据

```
val keys = Array("key1", "key2", "key3", "key4")
for(key <- keys){
  println(redisClient.get(key))
}
```


## 方法二、使用Redis pipeline批量读取Redis数据


### 导入相关库

```
import redis.clients.jedis.Pipeline
import redis.clients.jedis.Response
```

### 连接Redis

```
val redisHost = "localhost"
val redisPort = 8080
val redisClient = new Jedis(redisHost, redisPort)
redisClient.auth(redisPassword)
```
### 使用pipeline读取数据

```Scala
var tempRedisRes = Map[String, Response[String]]()
val keys = Array("key1", "key2", "key3", "key4")
val pp = redisClient.pipelined()
for(key <- keys){
  tempRedisRes ++= Map(key -> pp.get(key)) 
}
pp.sync()
```

### 增加连接Redis次数

为了防止连接Redis失败，需要增加尝试次数，在上面的代码外面增加一层逻辑，如下：

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

说明：`pp.get()`得到的是一个`Response[String]`的结果。


