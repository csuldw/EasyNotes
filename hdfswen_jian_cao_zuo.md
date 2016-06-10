# HDFS文件操作

## 删除

```
val output = new Path("hdfs://master:9000/output/")
val hdfs = org.apache.hadoop.fs.FileSystem.get(  
  new java.net.URI("hdfs://master:9000"), new org.apache.hadoop.conf.Configuration())  
// 删除输出目录  
if (hdfs.exists(output)) hdfs.delete(output, true)  
```