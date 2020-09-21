# Kafka 权威指南

## 初识 Kafka

## 安装 Kafka

* 执行范例中消费命令 kafka-console-consumer.sh --zookeeper localhost:2181 --topic test --from-beginning 报错为 [zookeeper is not a recognized option when executing kafka-console-consumer.sh](https://stackoverflow.com/a/53429129), 寻找到问题是 kafka 新版本(>0.9) 将消费命令更新为 kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning
* log.segment.bytes: 当日志片段大小达到此参数指定的上限（默认 1GB）时，当前日志就会被关闭，一个新的日志片段被打开。如果一个日志片段被关闭，就开始等待过期。这个参数的值越小，就会越频繁地关闭和分配新文件，从而降低磁盘写入的整体效率。如果主题的消息量不大，那么如何调整这个参数的大小就变得尤为重要。例如，如果一个主题每天只接收 100MB 的消息， 而 log.segment.bytes 使用默认设置， 那么需要 10 天时间才能填满一个日志片段。因为在日志片段被关闭之前消息是不会过期的，所以如果 log.retention.ms 被设为 604800000（也就是 1 周），那么日志片段最多需要 17 天才会过期。 这是因为关闭日志片段需要 10 天的时间，而根据配置的过期时间，还需要再保留 7 天时间（要等到日志片段里的最后一个消息过期才能被删除）。

