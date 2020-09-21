# Kafka 权威指南

## 初识 Kafka

## 安装 Kafka

* 执行范例中消费命令 kafka-console-consumer.sh --zookeeper localhost:2181 --topic test --from-beginning 报错为 [zookeeper is not a recognized option when executing kafka-console-consumer.sh](https://stackoverflow.com/a/53429129), 寻找到问题是 kafka 新版本(>0.9) 将消费命令更新为 kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning

