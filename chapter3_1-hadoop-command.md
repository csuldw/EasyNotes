# Hadoop笔记

## hadoop常用命令

常用命令`hadoop fs <args>`,以前有使用`hadoop dfs`,现在一般都是用`hadoop fs`，使用最广泛，可以操作任何的文件系统。

### 启动Hadoop

```
首先进入HADOOP_HOME目录
然后执行命令：sh bin/start-all.sh
```

### 关闭Hadoop

```
首先进入HADOOP_HOME目录
然后执行命令：sh bin/stop-all.sh
```

### 常用命令

```
hadoop fs -ls [hadoop folder]  #查看指定目录下内容
hadoop fs –cat [file PATH] #打开某个已存在文件
hadoop fs -put [local file PATH] [hadoop folder PATH] #将本地文件传至hadoop指定路径中
hadoop fs -put [local folder] [hadoop folder] #将本地目录传至hadoop文件系统目录
hadoop fs -get [hadoop file PATH] [local file PATH] #将hadoop上的文件下载到本地
hadoop fs -rm [hadoop file PATH] #删除hadoop上的指定文件
hadoop fs -rm [hadoop folder PATH] #删除hadoop上的指定目录
hadoop fs -mkdir [hadoop folder] #在hadoop中新建目录
hadoop fs -touchz [hadoop file PATH] #在hadoop中创建长度为0的空文件 
hadoop fs -mv [hadoop file1] [hadoop file2] #将hadoop中的file1命名为file2
hadoop fs -getmerge [hadoop folder] [filename] #将hadoop指定目录下所有内容保存为一个文件，同时down至本地
hadoop job –kill [job-id] #将正在运行的hadoop作业kill掉
hadoop fs -test -e [hadoop path] #判断hadoop中是否存在某个文件夹
hadoop fs -dus $uploadFilePath #查看目录下所有文件的总大小
```

下面是写在shell中的判断hdfs中是否存在某个文件夹，如果不存在，则创建

```
uploadPath="hdfs://localhost:9000/home/liudiwei/log/day"
hadoop fs -test -e $uploadPath
if [ $? -eq 1 ] ; then    
  hadoop fs -mkdir $uploadPath
fi
```