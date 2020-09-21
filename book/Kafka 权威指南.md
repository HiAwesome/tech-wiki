# Kafka 权威指南

## 初识 Kafka

## 安装 Kafka

* 执行范例中消费命令 kafka-console-consumer.sh --zookeeper localhost:2181 --topic test --from-beginning 报错为 [zookeeper is not a recognized option when executing kafka-console-consumer.sh](https://stackoverflow.com/a/53429129), 寻找到问题是 kafka 新版本(>0.9) 将消费命令更新为 kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning
* log.segment.bytes: 当日志片段大小达到此参数指定的上限（默认 1GB）时，当前日志就会被关闭，一个新的日志片段被打开。如果一个日志片段被关闭，就开始等待过期。这个参数的值越小，就会越频繁地关闭和分配新文件，从而降低磁盘写入的整体效率。如果主题的消息量不大，那么如何调整这个参数的大小就变得尤为重要。例如，如果一个主题每天只接收 100MB 的消息， 而 log.segment.bytes 使用默认设置， 那么需要 10 天时间才能填满一个日志片段。因为在日志片段被关闭之前消息是不会过期的，所以如果 log.retention.ms 被设为 604800000（也就是 1 周），那么日志片段最多需要 17 天才会过期。 这是因为关闭日志片段需要 10 天的时间，而根据配置的过期时间，还需要再保留 7 天时间（要等到日志片段里的最后一个消息过期才能被删除）。使用时间戳获取偏移量：日志片段的大小会影响使用时间戳获取偏移量。在使用时间戳获取日志偏移量时，Kafka 会检查分区里最后修改时间大于指定时间戳的日志片段（已经被关闭的），该日志片段的前一个文件的最后修改时间小于指定时间戳。然后，Kafka 返回该日志片段（也就是文件名）开头的偏移量。对于使用时间戳获取偏移量的操作来说，日志片段越小，结果越准确。
* log.segment.ms：另一个可以控制日志片段关闭时间的参数是 log.segment.ms，它指定了多长时间之后日志片段会被关闭。就像 log.retention.bytes 和 log.retention.ms 这两个参数一样，log.segment.bytes 和 log.retention.ms 这两个参数之间也不存在互斥问题。日志片段会在大小或时间达到上限时被关闭，就看哪个条件先得到满足。默认情况下，log.segment.ms 没有设定值，所以只根据大小来关闭日志片段。基于时间的日志片段对磁盘性能的影响：在使用基于时间的日志片段时，要着重考虑并行关闭多个日志片段对磁盘性能的影响。如果多个分区的日志片段永远不能达到大小的上限，就会发生这种情况，因为 broker 在启动之后就开始计算日志片段的过期时间，对于那些数据量小的分区来说，日志片段的关闭操作总是同时发生。
