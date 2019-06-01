# FAQ

```text
spark-shell --driver-memory 10G --executor-memory 10G --executor-cores 5 --num-executors 3 --jars
```

## Q1: Total size of serialized results of 40 tasks \(1041.3 MB\) is bigger than spark.driver.maxResultSize \(1024.0 MB\)

```bash
# Create new config
conf = (SparkConf()
    .set("spark.driver.maxResultSize", "2g"))
```

## Q2: java.util.concurrent.TimeoutException: Futures timed out after \[120 seconds\]

