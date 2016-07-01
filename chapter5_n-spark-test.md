## spark-shell测试命令

```
spark-shell --driver-memory 10G --executor-memory 10G --executor-cores 5 --num-executors 3 --jars deps/qbus-client-0.1.0.jar 
```

## serialized results of 9 tasks (1096.9 MB) is bigger than spark.driver.maxResultSize (1024.0 MB)

```
# Create new config
conf = (SparkConf()
    .set("spark.driver.maxResultSize", "2g"))
```

