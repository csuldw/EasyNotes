# Redis读取


import redis.clients.jedis.Jedis
import redis.clients.jedis.Pipeline
import redis.clients.jedis.Response

```
val redisClient = new Jedis("localhost", 9080)
redisClient.auth(redisPassword)
```