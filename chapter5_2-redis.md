# Redis操作

## sbt中添加redis

```
libraryDependencies += "redis.clients" % "jedis" % "2.6.2"
```

## 测试

需要引入redis.clients.jedis.Jedis库。


```
import redis.clients.jedis.Jedis
object RedisTest {
  def main(args: Array[String]) {
    val redisHost = "10.121.93.46"
    val redisPort = 9529
    val redisPassword = "dsgtwh"
    val redisClient = new Jedis(redisHost, redisPort, 0)
    redisClient.auth(redisPassword)
    val value = redisClient.get("1_218711")
    println(value)
    //val redisHost = config.getString("redisHost")
    //val redisPort = config.getInt("redisPort")
    //val redisPassword = config.getString("redisPassword")
    redisClient.close()
  }
```