# 测试用例


```
spark-shell --driver-memory 10G --executor-memory 10G --executor-cores 5 --num-executors 3 --jars deps/qbus-client-0.1.0.jar

val cfg = new Config()
val hdfsURLInput = "hdfsold://namenodefd1v.qss.zzzc.qihoo.net:9000"
val hdfsURLOutput = "hdfsold://namenodefd1v.qss.zzzc.qihoo.net:9000"

val hdfs = org.apache.hadoop.fs.FileSystem.get(new java.net.URI(hdfsURLOutput), new org.apache.hadoop.conf.Configuration())
val hdfsUtils = new HDFSFuncs(cfg)
val logPathPrefix = "/home/hdp-btime/project/web/stats"
val logPathDir = logPathPrefix +  "/2016-06-12"
val files = hdfsUtils.list(hdfsURLOutput,logPathDir)
val res = sc.parallelize(files)
val output = hdfsURLInput + "/home/hdp-btime/btime_rec_user_log/hdfs/history"
res.saveAsTextFile(output)
```

hadoop fs -rmr hdfs://namenodefd1v.qss.zzzc.qihoo.net:9000/home/hdp-btime/btime_rec_user_log/hdfs/history