# Kafka

## Windows

```shell
# 1、创建topic
kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test

# 2、打开一个producer
kafka-console-producer.bat --broker-list localhost:9092 --topic test

# 3、打开一个consumer
kafka-console-consumer.bat --zookeeper localhost:2181 --topic test

# 4、查看所有的topic
kafka-topics.bat --list --zookeeper localhost:2181

# 5、查询某个topic内的消息内容
kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic topicName --from-beginning
```

## ELK

>   https://www.cnblogs.com/niechen/p/10149962.htmlepkorg