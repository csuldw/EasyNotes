## Spark 实例

## sbt文件目录

    [liudiwei@localhost~/project/testDemo]$ tree -L 3
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

**注意**：默认情况下，如果没有`./src/main/scala`目录，那么使用sbt编译时，无法找到源码文件。

* build.sbt：用于compile


