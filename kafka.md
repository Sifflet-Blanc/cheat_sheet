[< Home](README.md)
# Kafka

Message broker. 

Asynchronous communication system.

2 main version :
- Kafka apache
- Kafka confluence


## Topics
Kafka is composed of topics, which are collections of messages.

Composed of one or more partitions. The partions are ordered, but the messages are not ordered between partitions.

To specify to our producer that we want to keep the same partition, we can set a key on a message. All messages with the same key will be push to the same partition.

In cli
```shell
/opt/kafka/bin/kafka-topics.sh --create --topic test \
--bootstrap-server localhost:9092 \
--partitions 3 \
--replication-factor 3 \
--config min.insync.replicas=2
```

## Producers
Kafka producers are used to sending messages to Kafka in a topic.

In cli
```shell
/opt/kafka/bin/kafka-console-producer.sh --topic test \
--bootstrap-server broker-1:9092,broker-2:9092,broker-3:9092 \
--producer-property partitioner.class=org.apache.kafka.clients.producer.RoundRobinPartitioner
```

## Consumers
Kafka consumers are used to reading messages from Kafka in a topic.

In cli
```shell
/opt/kafka/bin/kafka-console-consumer.sh --topic test --bootstrap-server broker-1:9092,broker-2:9092,broker-3:9092 --from-beginning --group group1
```

## in spring 
```yaml
<dependency>
    <groupId>org.springframework.kafka</groupId>
    <artifactId>spring-kafka</artifactId>
</dependency>
```