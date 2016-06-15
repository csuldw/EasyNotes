# shell 数值大小比较


```
-eq             //等于
-ne             //不等于
-gt             //大于
-lt             //小于
-ge            //大于等于
-le            //小于等于
```

## 判断shell程序是否存在

```
#!/bin/sh
count=`ps aux | grep -v grep | grep "sh test.sh" | wc -l`
echo $count
if [ $count -ge 1 ] ;
then
  echo "faild"
  exit
else
  sleep 40
fi
echo "end"
```