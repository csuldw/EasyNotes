## Spark 实例

## sbt文件目录

    .
    |-- build.sbt
    |-- conf
    |   `-- base.conf
    |-- deps
    |   `-- json4s-native_2.10-3.2.10.jar
    |-- project
    |   `-- target
    |       `-- config-classes
    |-- run.sh
    |-- src
    |   `-- main
    |       `-- scala
    |           `-- testDemo.scala 
    `-- target
        |-- resolution-cache
        |   |-- default
        |   `-- reports
        |-- scala-2.10
        |   |-- classes
        |   `-- testdemo_2.10-1.0.0.jar
        `-- streams
            |-- $global
            `-- compile

**注意**：默认情况下，如果没有`./src/main/scala`目录，那么使用sbt编译时，会找不到源文件，从而导致编译失败。

使用sbt编译时，只用到`build.sbt`、`src/main/scala/testDemo.scala`、`dep/*`几个文件，`target`和`project` 目录是执行`sbt compile`时生成的。

* build.sbt：用于`compile`时配置项目相关参数。
* 

