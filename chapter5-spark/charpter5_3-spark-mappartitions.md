# Spark算子-mapPartitions

mapPartitions和map很像，但map的对象是每个element，而mapPartitions是每个partition，需要注意的是mapPartitions里面的每个part都是一个iterator迭代器；

## Iterator误解

spark的mapPartitions算子，返回的是一个Iterator，如果mapPartitions中使用了map或是length，则会将Iterator全部消费完毕，致使Iterator的游标指向末尾，从而导致接下来的操作取不到数据。可行的方法就是，先将Iterator转换成Array保存起来，后面如果需要对part操作，只需要将Array再转换成Iterator就可以了。

```text
val cmbRedisRes = basicData.mapPartitions{
  part => {
    val partArr = part.toArray
    println("part count length: " + partArr.length)
    val keys = partArr.map(x => x._1).toSet.toArray
    println("The number of key is: " + keys.length)
    val partItr = partArr.toIterator
    partItr.map{
      x => x
    }
  }
}
```

如果直接使用`println("part count length: " + partArr.length)`，在使用`partItr.map`操作，则返回的结果为空。

