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