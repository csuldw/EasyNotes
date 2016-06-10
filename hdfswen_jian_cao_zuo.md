# HDFS文件操作

## 删除

```
val hdfs = org.apache.hadoop.fs.FileSystem.get(new java.net.URI("hdfs://namenodefd1v.qss.zzzc.qihoo.net:9000"), new org.apache.hadoop.conf.Configuration())
val output= new org.apache.hadoop.fs.Path("hdfsold://namenodefd1v.qss.zzzc.qihoo.net:9000/home/hdp-btime/btime_rec_user_log/app/20160610/right")
if (hdfs.exists(output)) hdfs.delete(output, true)  
```