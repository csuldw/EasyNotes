# Spark RDD操作-Action和Transformation



1. 输入：在Spark程序运行中，数据从外部数据空间（如分布式存储：textFile读取HDFS等，parallelize方法输入Scala集合或数据）输入Spark，数据进入Spark运行时数据空间，转化为Spark中的数据块，通过BlockManager进行管理。
2. 运行：在Spark数据输入形成RDD后便可以通过变换算子，如filter等，对数据进行操作并将RDD转化为新的RDD，通过Action算子，触发Spark提交作业。 如果数据需要复用，可以通过Cache算子，将数据缓存到内存。
3. 输出：程序运行结束数据会输出Spark运行时空间，存储到分布式存储中（如saveAsTextFile输出到HDFS），或Scala数据或集合中（collect输出到Scala集合，count返回Scala int型数据）。


Spark的核心数据模型是RDD，但RDD是个抽象类，具体由各子类实现，如MappedRDD、ShuffledRDD等子类。Spark将常用的大数据操作都转化成为RDD的子类。

## Transformation和Actions操作概况

### Transformation

- map(func) :返回一个新的分布式数据集，由每个原元素经过func函数转换后组成
- filter(func) : 返回一个新的数据集，由经过func函数后返回值为true的原元素组成
- flatMap(func) : 类似于map，但是每一个输入元素，会被映射为0到多个输出元素（因此，func函数的返回值是一个Seq，而不是单一元素）
- sample(withReplacement, frac, seed) :根据给定的随机种子seed，随机抽样出数量为frac的数据
- union(otherDataset) : 返回一个新的数据集，由原数据集和参数联合而成
- groupByKey([numTasks]) :在一个由（K,V）对组成的数据集上调用，返回一个（K，Seq[V])对的数据集。注意：默认情况下，使用8个并行任务进行分组，你可以传入numTask可选参数，根据数据量设置不同数目的Task
- reduceByKey(func, [numTasks]) : 在一个（K，V)对的数据集上使用，返回一个（K，V）对的数据集，key相同的值，都被使用指定的reduce函数聚合到一起。和groupbykey类似，任务的个数是可以通过第二个可选参数来配置的。
- join(otherDataset, [numTasks]) :在类型为（K,V)和（K,W)类型的数据集上调用，返回一个（K,(V,W))对，每个key中的所有元素都在一起的数据集
- groupWith(otherDataset, [numTasks]) : 在类型为（K,V)和(K,W)类型的数据集上调用，返回一个数据集，组成元素为（K, Seq[V], Seq[W]) Tuples。这个操作在其它框架，称为CoGroup
- cartesian(otherDataset) : 笛卡尔积。但在数据集T和U上调用时，返回一个(T，U）对的数据集，所有元素交互进行笛卡尔积。


### Actions

- reduce(func) : 通过函数func聚集数据集中的所有元素。Func函数接受2个参数，返回一个值。这个函数必须是关联性的，确保可以被正确的并发执行
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



