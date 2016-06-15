## HDFS文件操作

### 将Spark的数据结果存储到HDFS

```
val outPath = cfg.getString("hdfs://master:9000/home20160610/right")
output = new org.apache.hadoop.fs.Path(outPath)
......//RDD操作，返回一个rdd结果res
res.saveAsTextFile(output)
```

### 删除HDFS目录

```
import java.net.URI
import org.apache.hadoop.fs.{FileSystem, Path}
import org.apache.hadoop.conf.Configuration
def delete(hdfs_prefix : String, hdfs_path : String) : Boolean = {
  try {
    val conf = new Configuration()
    val fs = FileSystem.get(URI.create(hdfs_prefix), conf)
    val java_path = new Path(hdfs_path)
    val ret = fs.delete(java_path, true)
    ret
  } catch {
    case e : org.apache.commons.configuration.ConfigurationException =>
      println(e.getMessage)
      false
  }
}
```