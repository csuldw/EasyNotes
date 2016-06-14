# Spark RDD操作-Action和Transformation



1. input：在Spark程序运行中，当数据从外部数据空间（如分布式存储：使用textFile读取HDFS文件；parallelize方法输入Scala集合或数据）输入Spark时，数据就进入Spark运行时数据空间，并转化为Spark的数据块，通过BlockManager进行管理；
2. run：在Spark数据输入形成RDD之后便可进行算子变换，如filter，对数据进行操作并形成新的RDD，每当使用Action算子时，就会触发Spark提交作业，如果数据需要复用的话，可以通过Cache算子，将数据缓存到内存；
3. output：程序运行结束数据会输出Spark运行时空间，存储到分布式存储中（如saveAsTextFile输出到HDFS），或Scala数据或集合中（collect输出到Scala集合，count返回Scala int型数据）。


Spark的核心数据模型是RDD，但RDD是个抽象类，具体由各子类实现，如MappedRDD、ShuffledRDD等子类。Spark将常用的大数据操作都转化成为RDD的子类。

## Transformation和Actions操作概况

> Transformation的返回值还是一个RDD。它使用了链式调用的设计模式，对一个RDD进行计算后，变换成另外一个RDD，然后这个RDD又可以进行另外一次转换。这个过程是分布式的；
> Action返回值不是一个RDD。它要么是一个Scala的普通集合，要么是一个值，要么是空，最终或返回到Driver程序，或把RDD写入到文件系统中

### Transformation

transformation操作得到的是一个全新的RDD，方式很多，比如从数据源生成一个新的RDD，或者由一个RDD生成一个新的RDD。

**说明**：所有的transformation采用的都是懒（lazy）策略，即如果只提交transformation是不会执行计算的，计算只有在action被提交的时候才被触发。

- map(func)：对调用map的RDD中的每个element都使用func变换，并返回一个全新的分布式dataset；
- filter(func)：对原dataset使用func函数过滤，返回结果为true的dataset；
- flatMap(func)：类似于map，但是每一个输入元素，会被映射为0到多个输出元素（因此，func函数的返回值是一个Seq，而不是单一元素）；
- mapPartitions(func):和map很像，但map的对象是每个element，而mapPartitions是每个partition；  
- mapPartitionsWithSplit(func):和mapPartitions很像，但是func作用在其中一个split上，func需要有index；  
- sample(withReplacement, frac, seed)：抽样，根据给定的随机种子seed，随机抽样出数量为frac的数据；
- union(otherDataset)：返回一个新的dataset，由原dataset和参数联合而成；
- distinct([numTasks]):返回一个新的dataset，这个dataset含有的是原dataset中的distinct的element；  
- groupByKey([numTasks]) :在一个由（K,V）对组成的dataset上使用，返回一个（K，Seq[V])对的dataset。注意：默认情况下，使用8个并行任务进行分组，你可以传入numTask可选参数，根据数据量设置不同数目的Task；
- reduceByKey(func, [numTasks]) : 在一个（K，V)对的dataset上使用，返回一个（K，V）对的dataset，key相同的值，都被使用指定的reduce函数聚合到一起。和groupbykey类似，任务的个数是可以通过第二个可选参数来配置的；
- sortByKey([ascending],[numTasks]):按照key来进行排序，是升序还是降序，ascending是boolean类型；
- join(otherDataset, [numTasks]) :在类型为（K,V)和（K,W)类型的dataset上使用，返回一个（K,(V,W))对，每个key中的所有元素都在一起的数据集
- groupWith(otherDataset, [numTasks]) : 在类型为（K,V)和(K,W)类型的dataset上使用，返回一个新的dataset，元素组成为（K, Seq[V], Seq[W]) 的Tuples。这个操作在其它框架，称为CoGroup；
- cartesian(otherDataset) : 笛卡尔积。但在数据集T和U上调用时，返回一个(T，U）对的数据集，所有元素交互进行笛卡尔积。


### Actions

action操作得到的是一个值，或者说是一个结果（它会直接将RDD cache到内存中）。

- reduce(func) : 通过函数func聚集dataset中的所有元素。Func函数接受2个参数，返回一个值。这个函数必须是关联性的，确保可以被正确的并发执行；
- collect() : 在Driver的程序中，以数组的形式，返回数据集的所有元素。这通常会在使用filter或者其它操作后，返回一个足够小的数据子集再使用，直接将整个RDD集Collect返回，很可能会让Driver程序OOM
- count() : 返回数据集的元素个数
- take(n) : 返回一个数组，由数据集的前n个元素组成。注意，这个操作目前并非在多个节点上，并行执行，而是Driver程序所在机器，单机计算所有的元素(Gateway的内存压力会增大，需要谨慎使用）
- first() : 返回数据集的第一个元素（类似于take(1)）
- saveAsTextFile(path) : 将数据集的元素，以textfile的形式，保存到本地文件系统，hdfs或者任何其它hadoop支持的文件系统。Spark将会调用每个元素的toString方法，并将它转换为文件中的一行文本
- saveAsSequenceFile(path) : 将数据集的元素，以sequencefile的格式，保存到指定的目录下，本地系统，hdfs或者任何其它hadoop支持的文件系统。RDD的元素必须由key-value对组成，并都实现了Hadoop的Writable接口，或隐式可以转换为Writable（Spark包括了基本类型的转换，例如Int，Double，String等等）
- foreach(func) : 在数据集的每一个元素上，运行函数func。这通常用于更新一个累加器变量，或者和外部存储系统做交互


## 算子分类


大致可以分为三大类算子:

1. Value数据类型的Transformation算子，这种变换并不触发提交作业，针对处理的数据项是Value型的数据。
2. Key-Value数据类型的Transfromation算子，这种变换并不触发提交作业，针对处理的数据项是Key-Value型的数据对。
3. Action算子，这类算子会触发SparkContext提交Job作业。



