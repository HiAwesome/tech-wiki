# Java Performance Tuning

[Java性能权威指南（第2版）](https://book.douban.com/subject/35867531/)

## 工具

### CPU 使用率

```shell
vmstat 1
```

### 磁盘使用率

```shell
iostat -xm 5
```

### 网络使用率

```shell
nicstat 5
```

## Java 工具

### 运行时间

```shell
jcmd process_id VM.uptime
```

### 系统属性

```shell
jcmd process_id VM.system_properties
```

### JVM 版本

```shell
jcmd process_id VM.version
```

### JVM 命令行

```shell
jcmd process_id VM.command_line
```

### JVM 调优标志

```shell
jcmd process_id VM.flags [-all]
```

### JVM 平台属性默认值

```shell
java -XX:+PrintFlagsFinal -version
```







