# 判断程序是否存在

使用下列代码判断程序`sh test.sh`是否在后台执行!

```text
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

