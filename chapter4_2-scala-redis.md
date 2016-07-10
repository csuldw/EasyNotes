# Redis操作

## sbt中添加 Redis

```
libraryDependencies += "redis.clients" % "jedis" % "2.6.2"
```

## 测试

需要引入redis.clients.jedis.Jedis库。

```
import redis.clients.jedis.Jedis
object RedisTest {
  def main(args: Array[String]) {
    val redisHost = "localhost"
    val redisPort = 9529
    val redisPassword = "csuldw"
    val redisClient = new Jedis(redisHost, redisPort, 0)
    redisClient.auth(redisPassword)
    val value = redisClient.get("1_218711")
    println(value)
    redisClient.close()
  }
```




### redis set 

String redis.clients.jedis.Jedis.set(String key, String value, String nxxx, String expx, long time)
```
key 
value 
nxxx NX|XX, NX -- Only set the key if it does not already exist. XX 
-- Only set the key if it already exist.
expx EX|PX, expire time units: EX = seconds; PX = milliseconds
time expire time in the units of {@param #expx}
```