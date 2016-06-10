# HDFS文件操作

## 删除

```
val hdfs = org.apache.hadoop.fs.FileSystem.get(new java.net.URI("hdfs://namenodefd1v.qss.zzzc.qihoo.net:9000"), new org.apache.hadoop.conf.Configuration())
if (hdfs.exists(output)) hdfs.delete(output, true)  
```