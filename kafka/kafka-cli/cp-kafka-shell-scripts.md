# cp-kafka shell scripts

[https://medium.com/@TimvanBaarsen/apache-kafka-cli-commands-cheat-sheet-a6f06eac01b](https://medium.com/@TimvanBaarsen/apache-kafka-cli-commands-cheat-sheet-a6f06eac01b)

`alias docker-kafka-tools="docker run --rm -it  --dns-search=moscow.alfaintra.net infra.binary.alfabank.ru/cp-kafka:5.3.1"`

```
--- 
zookeeper-shell
    cluster membership
    controller election
    topic configuration
    access control lists
    quotas

Show the Kafka cluster id
docker-kafka-tools ./usr/bin/zookeeper-shell corpint5:5151 get cluster/id

list the broker in the Kafka cluster
docker-kafka-tools ./usr/bin/zookeeper-shell corpint5:5151 ls /brokers/ids

Show details of a Kafka broker
docker-kafka-tools ./usr/bin/zookeeper-shell corpint5:5151 get /brokers/ids/1001

Show all the topics that exist in the cluster
docker-kafka-tools ./usr/bin/zookeeper-shell corpint5:5151 ls /brokers/topics

Show details of a specific topic
docker-kafka-tools ./usr/bin/zookeeper-shell corpint5:5151 get /brokers/topics/SIMPLE
{"version":1,"partitions":{"2":[1002,1001,1004],"1":[1004,1002,1001],"0":[1001,1004,1002]}}
```

```
Create kafka topic
// no user in local docker kafka container
docker-kafka-tools ./usr/bin/kafka-topics --zookeeper corpint5:5151 --create --topic my-test-topic --partitions 3 --replication-factor 1

// in corpint5
// JMX connector server communication error: service:jmx:rmi://corpint5.moscow.alfaintra.net:9988 already used
[u_m15rt@corpint5 ~]$ docker exec -it kafka_um ./usr/bin/kafka-topics --zookeeper corpint5:5151 --create --topic my-test-topic-2 --partitions 3 --replication-factor 3

topic list
docker-kafka-tools ./usr/bin/kafka-topics --zookeeper corpint5:5151 --list --exclude-internal

topic explain
docker-kafka-tools ./usr/bin/kafka-topics --zookeeper corpint5:5151 --topic SIMPLE --describe

topic increase partitions
docker-kafka-tools ./usr/bin/kafka-topics --zookeeper corpint5:5151 --topic my-test-topic --alter --partitions 5

Change the retention time of a Kafka topic
// no auth from remote container
docker-kafka-tools ./usr/bin/kafka-configs --zookeeper corpint5:5151 --alter --entity-type topics --entity-name my-test-topic --add-config retention.ms=259200000

Purge a Kafka topic
At the time of writing, there is no single command to purge a topic.
As a workaround, you can purge a topic by changing the retention time of a topic to one minute.
--add-config retention.ms=1000

do not forget to return retention to default or apply old one
--delete-config retention.ms

Topic that overrides default configs
docker-kafka-tools ./usr/bin/kafka-topics --zookeeper corpint5:5151 --describe --topics-with-overrides

Certain topic overridden configs
docker-kafka-tools ./usr/bin/kafka-configs --zookeeper corpint5:5151 --describe --entity-type topics --entity-name my-test-topic

Delete topic
// admin
docker-kafka-tools ./usr/bin/kafka-topics --zookeeper corpint5:5151 --delete --topic my-test-topic

Find all the partitions where one or more of the replicas for the partition are not in-sync with the leader.
docker-kafka-tools ./usr/bin/kafka-topics --zookeeper corpint5:5151 --describe --under-replicated-partitions
```
