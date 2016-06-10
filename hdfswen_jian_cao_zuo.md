## HDFS文件操作

### 存储数据至HDFS

```
val outPath = cfg.getString("hdfs://master:9000/home20160610/right")
output = new org.apache.hadoop.fs.Path(outPath)
......//RDD操作，返回一个rdd结果res
res.saveAsTextFile(output)
```


### 删除HDFS目录

```
def delete(hdfs_prefix : String, hdfs_path : String) : Boolean = {
  try {
    val conf = new org.apache.hadoop.conf.Configuration()
    val fs = FileSystem.get(java.net.URI.create(hdfs_prefix), conf)
    val java_path = new org.apache.hadoop.fs.Path(hdfs_path)
    val ret = fs.delete(java_path, true)
    ret
  } catch {
    case e : org.apache.commons.configuration.ConfigurationException =>
      println(e.getMessage)
      false
  }
}
```