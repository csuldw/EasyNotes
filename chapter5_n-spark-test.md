## spark-shell测试命令

```
spark-shell --driver-memory 10G --executor-memory 10G --executor-cores 5 --num-executors 3 --jars deps/qbus-client-0.1.0.jar 
```

## Total size of serialized results of 40 tasks (1041.3 MB) is bigger than spark.driver.maxResultSize (1024.0 MB)

```
# Create new config
conf = (SparkConf()
    .set("spark.driver.maxResultSize", "2g"))
```

> hdfs://namenodefd1v.qss.zzzc.qihoo.net:9000/home/hdp-btime/btime_rec_user_log/hdfs/web/20160623/2016-06-23-18-42-04


