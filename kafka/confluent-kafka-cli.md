# Confluent kafka cli

[https://developer.confluent.io/tutorials/kafka-console-consumer-producer-basics/kafka.html](https://developer.confluent.io/tutorials/kafka-console-consumer-producer-basics/kafka.html)

```
---
version: '3'
services:
  zoo1:
    image: confluentinc/cp-zookeeper:7.0.1
    container_name: zookeeper
    environment:
      ZOOKEEPER_CLIENT_PORT: 2181
      ZOOKEEPER_TICK_TIME: 2000

  kafka1:
    image: confluentinc/cp-kafka:7.0.1
    container_name: broker
    ports:
    # To learn about configuring Kafka for access across networks see
    # https://www.confluent.io/blog/kafka-client-cannot-connect-to-broker-on-aws-on-docker-etc/
      - "9092:9092"
    depends_on:
      - zookeeper
    environment:
      KAFKA_BROKER_ID: 1
      KAFKA_ZOOKEEPER_CONNECT: 'zookeeper:2181'
      KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: PLAINTEXT:PLAINTEXT,PLAINTEXT_INTERNAL:PLAINTEXT
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://localhost:9092,PLAINTEXT_INTERNAL://kafka1:29092
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
      KAFKA_TRANSACTION_STATE_LOG_MIN_ISR: 1
      KAFKA_TRANSACTION_STATE_LOG_REPLICATION_FACTOR: 1
      
```



```
docker-compose up -d
```

### Подключиться к контейнеру

[https://www.baeldung.com/kafka-docker-connection#client-connecting-from-a-different-host](https://www.baeldung.com/kafka-docker-connection#client-connecting-from-a-different-host)

```
docker run --rm --net=kafka_default confluentinc/cp-kafka:7.0.1 \
    kafka-topics \
    --bootstrap-server kafka1:19092 --list
    
// --net=kafka_default  имя сети
// kafka1:19092 из KAFKA_ADVERTISED_LISTENERS: LISTENER_DOCKER_INTERNAL://kafka1:19092
```

```
// к удаленному контейнеру
docker run --rm -it  --dns-search=moscow.alfaintra.net infra.binary.alfabank.ru/cp-kafka:5.3.1
```

```
docker exec container_name kafka-topics ...parameters
```

```
docker-compose exec service_name kafka-topics ...parameters
```

### create topic, produce and consume

```
kafka-topics --bootstrap-server kafka1:9092 \
    --create \
    --topic TEST
    
kafka-console-producer --bootstrap-server kafka1:9092 \
    --topic TEST
{...messges...}

kafka-console-consumer --bootstrap-server kafka1:9092 \
    --topic TEST \
    --from-begining
```

```
docker exec -it kafka1 kafka-console-consumer --bootstrap-server kafka1:9092 --from-beginning --topic NPE-1 > NPE
```

```
kafka-topics --create --topic example-topic --bootstrap-server kafka1:9092 --replication-factor 1 --partitions 2

kafka-console-producer --topic example-topic --bootstrap-server kafka1:9092 \
  --property parse.key=true \
  --property key.separator=":"

key1:the lazy
key2:fox jumped
key3:over the
key4:brown cow
key1:All
key2:streams
key3:lead
key4:to
key1:Kafka
key2:Go to
key3:Kafka
key4:summit

kafka-console-consumer --topic example-topic --bootstrap-server kafka1:9092 \
 --from-beginning \
 --property print.key=true \
 --property key.separator="-" \
 --partition 0
 
key1-the lazy
key1-All
key1-Kafka

kafka-console-consumer --topic example-topic --bootstrap-server kafka1:9092 \
 --property print.key=true \
 --property key.separator="-" \
 --partition 1 \
 --offset 6
```



### Выставить офсет

```
docker run -it --name reset-offset --rm \
    --dns-search=moscow.alfaintra.net \
    wurstmeister/kafka \
    /opt/kafka/bin/kafka-consumer-groups.sh \
    --bootstrap-server corpint4:9092,corpint5:9092,corpint7:9092 \
    --group async_printer_worker_consumer_01 \
    --delete

docker run -it --name reset-offset --rm \
    --dns-search=moscow.alfaintra.net \
    wurstmeister/kafka \
    /opt/kafka/bin/kafka-consumer-groups.sh \
    --bootstrap-server corpint4:9092,corpint5:9092,corpint7:9092 \
    --group async_printer_worker_consumer_01 \
    --topic PRINT_TASKS_V01 \
    --reset-offsets \
    --to-latest | --to-offset 1000 | --to-earliest \
    --execute
```

### kafka-topics

```
kafka-topics --bootstrap-server kafka1:9092 --create \
    --topic TEST \
    --partitions 3 \
    --replication-factor 1
```

```
kafka-topics --bootstrap-server kafka1:9092 --alter \
    --topic TEST \
    --partitions 9
```

```
kafka-topics --bootstrap-server kafka1:9092 --list
```

```
kafka-topics --bootstrap-server kafka1:9092 \
    --topic TEST --describe
```

```
kafka-topics --bootstrap-server kafka1:9092 \
    --topic TEST --delete
    
Помеченные на удаление топики
// zkCli.sh
ls /admin/delete_topics
```



### Purge data

```
kafka-topics --bootstrap-server kafka1:9092 \
    --topic TEST \
    --alter \
    --config retention.ms=1000
```

```
kafka-configs --bootstrap-server kafka1:9092 \
    --entity-type topics \
    --entity-name TEST \
    --alter --add-config retention.ms=1000
```

```
kafka-configs --bootstrap-server kafka1:9092 \
    --entity-type topics \
    --entity-name TEST \
    --describe
```

### perfomance test

```
kafka-producer-perf-test --producer-props bootstrap.servers=kafka1:9092 --topic TEST \
    --record-size 1000 \
    --throughput 1000 \
    --num-records 3600000
```



Find all the partitions where one or more of the replicas for the partition are not in-sync with the leader.

```
kafka-topics --zookeeper corpint5:5151 --describe --under-replicated-partitions
```



### zookeeper-shell

```
docker exec kafka1 zookeeper-shell zoo1:2181 get /cluster/id
```

```
//list the broker in the Kafka cluster
zookeeper-shell zoo1:2181 ls /brokers/ids
```

```
// Show details of a Kafka broker
zookeeper-shell corpint5:5151 get /brokers/ids/1001
```

```
// Show all the topics that exist in the cluster
zookeeper-shell corpint5:5151 ls /brokers/topics
```

```
// Show details of a specific topic
zookeeper-shell corpint5:5151 get /brokers/topics/SIMPLE
{"version":1,"partitions":{"2":[1002,1001,1004],"1":[1004,1002,1001],"0":[1001,1004,1002]}}
```
