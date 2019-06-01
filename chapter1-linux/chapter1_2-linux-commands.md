# 常用命令

## awk按指定字符切割

```text
cat 20160524_v1_0.log | awk -F 'uid\":\"' '{print $2}'  | awk -F '\"' '{print $1}' > uid_v1.data
```

## 文件内容按行去重

```text
cat text.file | sort -u > text.out
```

## 对文件按指定条件过滤

```text
cat uid_v1.uniq | awk '{if(length($0)==32) print $0 }' | grep -v "=" > uid_v1.uniq2
```

## 两个文件取交集

```text
cat file1 file2 | sort -u | uniq -d
```

## 获取当前时间的前一小时

```text
date -d "1 hour ago" +"%Y%m%d%H"
```

## 判断文件夹是否存在，不存在则创建

```text
if [ ! -d "$savePath" ];then
   mkdir "$savePath"
fi
```

## 判断文件是否存在

```text
if [ ! -f "$savefile" ];then
```

## 变量值减一

```text
hour=3
hour2=`expr $hour - 1`
```

## zip解压

```text
gunzip -c *.gz > output.data
```

## 获取文件大小

```text
ls -l $localFilePath | awk '{ print $5 }'
```

## 获取hadoop中的某个文件大小

```text
return_str="`hadoop fs -dus $uploadFilePath`"
hdpsize=`echo $return_str | cut -d \  -f 2`
```

## 大小比较

```text
-eq             //等于
-ne             //不等于
-gt             //大于
-lt             //小于
-ge            //大于等于
-le            //小于等于
```

## 分组求和

> awk '{a\[$1\]+=$2;b\[$1\]+=$3}END{for\(i in a\)print i,a\[i\],b\[i\]}' msg.3

