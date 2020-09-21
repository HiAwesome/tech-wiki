# Kafka 权威指南

## 初识 Kafka

## 安装 Kafka

* 执行范例中消费命令 kafka-console-consumer.sh --zookeeper localhost:2181 --topic test --from-beginning 报错为 [zookeeper is not a recognized option when executing kafka-console-consumer.sh](https://stackoverflow.com/a/53429129), 寻找到问题是 kafka 新版本(>0.9) 将消费命令更新为 kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning

## Kafka 生产者

* log.segment.bytes: 当日志片段大小达到此参数指定的上限（默认 1GB）时，当前日志就会被关闭，一个新的日志片段被打开。如果一个日志片段被关闭，就开始等待过期。这个参数的值越小，就会越频繁地关闭和分配新文件，从而降低磁盘写入的整体效率。如果主题的消息量不大，那么如何调整这个参数的大小就变得尤为重要。例如，如果一个主题每天只接收 100MB 的消息， 而 log.segment.bytes 使用默认设置， 那么需要 10 天时间才能填满一个日志片段。因为在日志片段被关闭之前消息是不会过期的，所以如果 log.retention.ms 被设为 604800000（也就是 1 周），那么日志片段最多需要 17 天才会过期。 这是因为关闭日志片段需要 10 天的时间，而根据配置的过期时间，还需要再保留 7 天时间（要等到日志片段里的最后一个消息过期才能被删除）。使用时间戳获取偏移量：日志片段的大小会影响使用时间戳获取偏移量。在使用时间戳获取日志偏移量时，Kafka 会检查分区里最后修改时间大于指定时间戳的日志片段（已经被关闭的），该日志片段的前一个文件的最后修改时间小于指定时间戳。然后，Kafka 返回该日志片段（也就是文件名）开头的偏移量。对于使用时间戳获取偏移量的操作来说，日志片段越小，结果越准确。
* log.segment.ms：另一个可以控制日志片段关闭时间的参数是 log.segment.ms，它指定了多长时间之后日志片段会被关闭。就像 log.retention.bytes 和 log.retention.ms 这两个参数一样，log.segment.bytes 和 log.retention.ms 这两个参数之间也不存在互斥问题。日志片段会在大小或时间达到上限时被关闭，就看哪个条件先得到满足。默认情况下，log.segment.ms 没有设定值，所以只根据大小来关闭日志片段。基于时间的日志片段对磁盘性能的影响：在使用基于时间的日志片段时，要着重考虑并行关闭多个日志片段对磁盘性能的影响。如果多个分区的日志片段永远不能达到大小的上限，就会发生这种情况，因为 broker 在启动之后就开始计算日志片段的过期时间，对于那些数据量小的分区来说，日志片段的关闭操作总是同时发生。
* 顺序保证：Kafka可以保证同一个分区里的消息是有序的。也就是说，如果生产者按照一定的顺序发送消息，broker就会按照这个顺序把它们写入分区，消费者也会按照同样的顺序读取它们。在某些情况下，顺序是非常重要的。例如，往一个账户存入100元再取出来，这个与先取钱再存钱是截然不同的！不过，有些场景对顺序不是很敏感。如果把retries设为非零整数，同时把max.in.flight.requests.per.connection设为比1大的数，那么，如果第一个批次消息写入失败，而第二个批次写入成功，broker会重试写入第一个批次。如果此时第一个批次也写入成功，那么两个批次的顺序就反过来了。一般来说，如果某些场景要求消息是有序的，那么消息是否写入成功也是很关键的，所以不建议把retries设为0。可以把max.in.flight.requests.per.connection设为1，这样在生产者尝试发送第一批消息时，就不会有其他的消息发送给broker。不过这样会严重影响生产者的吞吐量，所以只有在对消息的顺序有严格要求的情况下才能这么做。
* schema.registry.url 的问题，参考 [How to set schema.registry.URL?](https://stackoverflow.com/a/51904064), 先从这里 [Confluent Platform](https://www.confluent.io/download/) 下载一个 Self managed event streaming platform, 填写 email，选择手动部署，选择 tar，最终下载一个类似名为 confluent-5.5.1-2.12.tar.gz 的文件，然后解压，进入文件夹 etc/schema-registry/schema-registry.properties 设定 listeners=http://localhost:8081, 然后返回文件夹顶级目录运行 bin/schema-registry-start etc/schema-registry/schema-registry.properties 即可。
* 分区：如果键值为 null，并且使用了默认的分区器，那么记录将被随机地发送到主题内各个可用的分区上。分区器使用轮询（Round Robin）算法将消息均衡地分布到各个分区上。如果键不为空，并且使用了默认的分区器，那么 Kafka 会对键进行散列（使用Kafka自己的散列算法，即使升级 Java 版本，散列值也不会发生变化），然后根据散列值把消息映射到特定的分区上。

## Kafka 消费者

* 分配分区是怎样的一个过程：当消费者要加入群组时，它会向群组协调器发送一个 JoinGroup 请求。第一个加入群组的消费者将成为“群主”。群主从协调器那里获得群组的成员列表（列表中包含了所有最近发送过心跳的消费者，它们被认为是活跃的），并负责给每一个消费者分配分区。它使用一个实现了 PartitionAssignor 接口的类来决定哪些分区应该被分配给哪个消费者。




