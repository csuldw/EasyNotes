## HDFS文件操作

### 存储数据至HDFS

```
val hdfs = org.apache.hadoop.fs.FileSystem.get(new java.net.URI("hdfs://master:9000"), new org.apache.hadoop.conf.Configuration())
hdfs.saveAsTextFile()
```


### 删除HDFS目录

```
val hdfs = org.apache.hadoop.fs.FileSystem.get(new java.net.URI("hdfs://master:9000"), new org.apache.hadoop.conf.Configuration())
val output= new org.apache.hadoop.fs.Path("hdfs://master:9000/home20160610/right")
if (hdfs.exists(output)) hdfs.delete(output, true)  
```