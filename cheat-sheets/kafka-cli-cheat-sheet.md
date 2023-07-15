# Kafka cli cheat sheet

### Contents

* [Dev Environment Setup in Docker](https://thecodinginterface.com/blog/kafka-cli-cheat-sheet/#setup-docker-env)
* [Creating, Listing, and Describing Kafka Topics](https://thecodinginterface.com/blog/kafka-cli-cheat-sheet/#create-list-describe-topics)
* [Basic Schemaless Message Producing and Consuming](https://thecodinginterface.com/blog/kafka-cli-cheat-sheet/#schemaless-pub-sub)
* [View and Reset Consumer Offsets](https://thecodinginterface.com/blog/kafka-cli-cheat-sheet/#consumer-offsets)
* [View the Amount of Data in a Topic](https://thecodinginterface.com/blog/kafka-cli-cheat-sheet/#amount-of-topic-data)
* [Consuming from a Specific Offset and Number of Messages](https://thecodinginterface.com/blog/kafka-cli-cheat-sheet/#consumer-seeking)
* [Deleting a Topic](https://thecodinginterface.com/blog/kafka-cli-cheat-sheet/#delete-topic)
* [Producing and Consuming Avro Messages](https://thecodinginterface.com/blog/kafka-cli-cheat-sheet/#avro-pub-sub)
* [Resources for Further Learning](https://thecodinginterface.com/blog/kafka-cli-cheat-sheet/#learn-more)

### Dev Environment Setup in Docker <a href="#setup-docker-env" id="setup-docker-env"></a>

To facilitate experimentation and verification of commands in a low effort / low impact environment I'llprovide the following docker-compose.yml definition utilizing publicly available Confluent community Docker images for Kafka, Zookeeper, and Schema Registry.

```python
---
version: '2'
services:
  zookeeper:
    image: confluentinc/cp-zookeeper:6.1.1
    hostname: zookeeper
    container_name: zookeeper
    ports:
      - "2181:2181"
    environment:
      ZOOKEEPER_CLIENT_PORT: 2181
      ZOOKEEPER_TICK_TIME: 2000

  broker:
    image: confluentinc/cp-kafka:6.1.1
    hostname: broker
    container_name: broker
    depends_on:
      - zookeeper
    ports:
      - "29092:29092"
      - "9092:9092"
    environment:
      KAFKA_BROKER_ID: 1
      KAFKA_ZOOKEEPER_CONNECT: 'zookeeper:2181'
      KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: PLAINTEXT:PLAINTEXT,PLAINTEXT_HOST:PLAINTEXT
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://broker:29092,PLAINTEXT_HOST://localhost:9092
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
      KAFKA_TRANSACTION_STATE_LOG_MIN_ISR: 1
      KAFKA_TRANSACTION_STATE_LOG_REPLICATION_FACTOR: 1

  schema-registry:
    image: confluentinc/cp-schema-registry:6.1.1
    hostname: schema-registry
    container_name: schema-registry
    depends_on:
      - broker
    ports:
      - "8081:8081"
    environment:
      SCHEMA_REGISTRY_HOST_NAME: schema-registry
      SCHEMA_REGISTRY_KAFKASTORE_BOOTSTRAP_SERVERS: 'broker:29092'
      SCHEMA_REGISTRY_LISTENERS: 
```

Fire up the Docker Compose services with the following.

```python
docker-compose up
```

This will produce the following docker services.

```python
$ docker-compose ps
     Name                  Command            State                        Ports
------------------------------------------------------------------------------------------------------
broker            /etc/confluent/docker/run   Up      0.0.0.0:29092->29092/tcp, 0.0.0.0:9092->9092/tcp
schema-registry   /etc/confluent/docker/run   Up      0.0.0.0:8081->8081/tcp
zookeeper         /etc/confluent/docker/run   Up      0.0.0.0:2181->2181/tcp, 2888/tcp, 3888/tcp
```

I'll be accessing the Kafka CLI tools from these running docker containers.\
To exec into the broker container use the following.

```python
docker exec -it broker /bin/bash
```

Similarly to exec into the schema-registry container use the following.

```python
docker exec -it schema-registry /bin/bash
```

### Creating, Listing, and Describing Kafka Topics <a href="#create-list-describe-topics" id="create-list-describe-topics"></a>

\
Create people topic with 3 partitions from within broker container.

```python
kafka-topics --bootstrap-server localhost:9092 --create --topic people --replication-factor 1 --partitions 3
```

Create pets topic with 1 partition from within broker container.

```python
kafka-topics --bootstrap-server localhost:9092 --create --topic pets --replication-factor 1 --partitions 1
```

List topics from within broker container

```python
kafka-topics --bootstrap-server localhost:9092 --list
```

Output.

```python
__consumer_offsets
_schemas
people
pets
```

&#x20;

Describe the people topic.

```python
kafka-topic --bootstrap-server localhost:9092 --describe --topic people
```

Output.

```python
Topic: people	PartitionCount: 3	ReplicationFactor: 1	Configs:
	Topic: people	Partition: 0	Leader: 1	Replicas: 1	Isr: 1
	Topic: people	Partition: 1	Leader: 1	Replicas: 1	Isr: 1
	Topic: people	Partition: 2	Leader: 1	Replicas: 1	Isr: 1
```

To create a topic with a specific retention policy and/or as a compacted topic.

```python
kafka-topics --bootstrap-server localhost:9092 --create --topic people.promotions --partitions 1 --replication-factor 1 \
  --config retention.ms=360000 --config cleanup.policy=compact
```

To get a detailed look at al the configs that are used by a particular topic using the kafka-configs CLI tool.

```python
kafka-configs --bootstrap-server localhost:9092 --describe --all --topic people.promotions
```

Output.

```python
All configs for topic people.promotions are:
  compression.type=producer sensitive=false synonyms={DEFAULT_CONFIG:compression.type=producer}
  leader.replication.throttled.replicas= sensitive=false synonyms={}
  message.downconversion.enable=true sensitive=false synonyms={DEFAULT_CONFIG:log.message.downconversion.enable=true}
  min.insync.replicas=2 sensitive=false synonyms={STATIC_BROKER_CONFIG:min.insync.replicas=2, DEFAULT_CONFIG:min.insync.replicas=1}
  segment.jitter.ms=0 sensitive=false synonyms={}
  cleanup.policy=compact sensitive=false synonyms={DYNAMIC_TOPIC_CONFIG:cleanup.policy=compact, DEFAULT_CONFIG:log.cleanup.policy=delete}
  flush.ms=9223372036854775807 sensitive=false synonyms={}
  follower.replication.throttled.replicas= sensitive=false synonyms={}
  segment.bytes=1073741824 sensitive=false synonyms={DEFAULT_CONFIG:log.segment.bytes=1073741824}
  retention.ms=360000 sensitive=false synonyms={DYNAMIC_TOPIC_CONFIG:retention.ms=360000}
  flush.messages=9223372036854775807 sensitive=false synonyms={DEFAULT_CONFIG:log.flush.interval.messages=9223372036854775807}
  message.format.version=2.7-IV2 sensitive=false synonyms={DEFAULT_CONFIG:log.message.format.version=2.7-IV2}
  max.compaction.lag.ms=9223372036854775807 sensitive=false synonyms={DEFAULT_CONFIG:log.cleaner.max.compaction.lag.ms=9223372036854775807}
  file.delete.delay.ms=60000 sensitive=false synonyms={DEFAULT_CONFIG:log.segment.delete.delay.ms=60000}
  max.message.bytes=1048588 sensitive=false synonyms={DEFAULT_CONFIG:message.max.bytes=1048588}
  min.compaction.lag.ms=0 sensitive=false synonyms={DEFAULT_CONFIG:log.cleaner.min.compaction.lag.ms=0}
  message.timestamp.type=CreateTime sensitive=false synonyms={DEFAULT_CONFIG:log.message.timestamp.type=CreateTime}
  preallocate=false sensitive=false synonyms={DEFAULT_CONFIG:log.preallocate=false}
  min.cleanable.dirty.ratio=0.5 sensitive=false synonyms={DEFAULT_CONFIG:log.cleaner.min.cleanable.ratio=0.5}
  index.interval.bytes=4096 sensitive=false synonyms={DEFAULT_CONFIG:log.index.interval.bytes=4096}
  unclean.leader.election.enable=false sensitive=false synonyms={DEFAULT_CONFIG:unclean.leader.election.enable=false}
  retention.bytes=-1 sensitive=false synonyms={DEFAULT_CONFIG:log.retention.bytes=-1}
  delete.retention.ms=86400000 sensitive=false synonyms={DEFAULT_CONFIG:log.cleaner.delete.retention.ms=86400000}
  segment.ms=604800000 sensitive=false synonyms={}
  message.timestamp.difference.max.ms=9223372036854775807 sensitive=false synonyms={DEFAULT_CONFIG:log.message.timestamp.difference.max.ms=9223372036854775807}
  segment.index.bytes=10485760 sensitive=false synonyms={DEFAULT_CONFIG:log.index.size.max.bytes=10485760}
```

### Basic Schemaless Message Producing and Consuming <a href="#schemaless-pub-sub" id="schemaless-pub-sub"></a>

From the broker container publish some people data.

```python
kafka-console-producer --bootstrap-server localhost:9092 --topic people
>{"name":"Stewie", "show": "Family Guy"}
>{"name":"Popeye", "show": "Popeye"}
>{"name":"Bart", "show": "The Simpsons"}
>{"name":"Fred", "show": "The Flintstones"}
>{"name":"Shaggy", "show": "Scooby-Doo"}
```

_CTRL+C to exit producer session_

From the broker container publish some pets with an explicit key separated by a pipe character.

```python
kafka-console-producer --bootstrap-server localhost:9092 --topic pets --property "parse.key=true" --property "key.separator=|"
>baxter|{"name": "Baxter", "movie": "Anchorman"}
>marley|{"name": "Marley", "movie": "Marley and Me"}
>toothless|{"name": "Toothless", "movie": "How To Train Your Dragon"}
>willy|{"name": "Willy", "movie": "Free Willy"}
>babe|{"name": "Babe", "movie": "Babe"}
>hooch|{"name": "Hooch", "movie": "Turner & Hooch"}
```

From the broker container consume all people messages from the beginning.

```python
kafka-console-consumer --bootstrap-server localhost:9092 --topic people --from-beginning
```

Output.

```python
{"name":"Stewie", "show": "Family Guy"}
{"name":"Fred", "show": "The Flintstones"}
{"name":"Bart", "show": "The Simpsons"}
{"name":"Shaggy", "show": "Scooby-Doo"}
{"name":"Popeye", "show": "Popeye"}
^CProcessed a total of 5 messages
```

_CTRL+C to exit producer session_

From broker container consume all pet messages and print the key of messages.

```python
kafka-console-consumer --bootstrap-server localhost:9092 --topic pets --from-beginning --property print.key=true
```

Output.

```python
baxter	{"name": "Baxter", "movie": "Anchorman"}
marley	{"name": "Marley", "movie": "Marley and Me"}
toothless	{"name": "Toothless", "movie": "How To Train Your Dragon"}
willy	{"name": "Willy", "movie": "Free Willy"}
babe	{"name": "Babe", "movie": "Babe"}
hooch	{"name": "Hooch", "movie": "Turner & Hooch"}
```

&#x20;

Specify an explicit consumer group id while consuming from pets topic and print each messages offset.

```python
kafka-console-consumer --bootstrap-server localhost:9092 --topic pets --from-beginning \
    --group adam-pet-consumer --property print.offset=true
```

Output.

```python
Offset:0	{"name": "Baxter", "movie": "Anchorman"}
Offset:1	{"name": "Marley", "movie": "Marley and Me"}
Offset:2	{"name": "Toothless", "movie": "How To Train Your Dragon"}
Offset:3	{"name": "Willy", "movie": "Free Willy"}
Offset:4	{"name": "Babe", "movie": "Babe"}
Offset:5	{"name": "Hooch", "movie": "Turner & Hooch"}
```

Specify an explicit consumer group id for people topic and print message timestamp.

```python
kafka-console-consumer --bootstrap-server localhost:9092 --topic people --from-beginning \
    --group adam-people-consumer --property print.timestamp=true
```

Output.

```python
CreateTime:1645938589986	{"name":"Stewie", "show": "Family Guy"}
CreateTime:1645938688396	{"name":"Fred", "show": "The Flintstones"}
CreateTime:1645938656642	{"name":"Bart", "show": "The Simpsons"}
CreateTime:1645938735597	{"name":"Shaggy", "show": "Scooby-Doo"}
CreateTime:1645938624686	{"name":"Popeye", "show": "Popeye"}
```

&#x20;

### View and Reset Consumer Offsets <a href="#consumer-offsets" id="consumer-offsets"></a>

The \_\_consumer\_offsets topic stores the message offset that each consumer group should consume next. In the broker container run the following to get a look at where each consumer group is at for each topic partition.

```python
kafka-consumer-groups --bootstrap-server localhost:9092 --describe --all-groups
```

Output.

```python
Consumer group 'adam-people-consumer' has no active members.

GROUP                TOPIC           PARTITION  CURRENT-OFFSET  LOG-END-OFFSET  LAG             CONSUMER-ID     HOST            CLIENT-ID
adam-people-consumer people          0          2               2               0               -               -               -
adam-people-consumer people          2          2               2               0               -               -               -
adam-people-consumer people          1          1               1               0               -               -               -

Consumer group 'adam-pet-consumer' has no active members.

GROUP             TOPIC           PARTITION  CURRENT-OFFSET  LOG-END-OFFSET  LAG             CONSUMER-ID     HOST            CLIENT-ID
adam-pet-consumer pets            0          6               6               0               -               -               -
```

&#x20;

Or do this for a specific group like adam-people-consumer.

```python
kafka-consumer-groups --bootstrap-server localhost:9092 --describe --group adam-people-consumer
```

Output.

```python
Consumer group 'adam-people-consumer' has no active members.

GROUP                TOPIC           PARTITION  CURRENT-OFFSET  LOG-END-OFFSET  LAG             CONSUMER-ID     HOST            CLIENT-ID
adam-people-consumer people          0          2               2               0               -               -               -
adam-people-consumer people          2          2               2               0               -               -               -
adam-people-consumer people          1          1               1               0               -               -               -
```

Reset the adam-pet-consumer group to the beginning. Can use --dry-run to test the action of the reset or --execute to make reset permanent.

```python
kafka-consumer-groups --bootstrap-server localhost:9092 --group adam-pet-consumer \
    --reset-offsets --to-earliest --topic people --dry-run
```

Output.

```python
GROUP                          TOPIC                          PARTITION  NEW-OFFSET
adam-pet-consumer              people                         0          0
adam-pet-consumer              people                         1          0
adam-pet-consumer              people                         2          0
```

Then do it for real.

```python
kafka-consumer-groups --bootstrap-server localhost:9092 --group adam-pet-consumer \
    --reset-offsets --to-earliest --topic people --execute
```

Can also specify the specific topic and partition to reset a offset to. For example to reset adam-people-consumer group people topic partition 0 to an offset of 1 would be as follows.

```python
kafka-consumer-groups --bootstrap-server localhost:9092 --group adam-people-consumer \
    --reset-offsets --to-offset 1 --topic people:0 --execute
```

Output.

```python
GROUP                          TOPIC                          PARTITION  NEW-OFFSET
adam-people-consumer           people                         0          1
```

Yet another way to view the consumer offsets is to fetch the contents of \_\_consumer\_offsets

```python
kafka-console-consumer --bootstrap-server localhost:9092 --from-beginning --topic __consumer_offsets --formatter "kafka.coordinator.group.GroupMetadataManager\$OffsetsMessageFormatter"
```

### Viewing the Amount of Data in a Topic <a href="#amount-of-topic-data" id="amount-of-topic-data"></a>

In broker container run this to see how many bytes are in people topic.

```python
kafka-log-dirs --bootstrap-server localhost:9092 --describe --topic-list people
```

Output.

```python
Querying brokers for log directories information
Received log directory information from brokers 1
{"version":1,"brokers":[{"broker":1,"logDirs":[{"logDir":"/var/lib/kafka/data","error":null,"partitions":[{"partition":"people-0","size":217,"offsetLag":0,"isFuture":false},{"partition":"people-2","size":214,"offsetLag":0,"isFuture":false},{"partition":"people-1","size":103,"offsetLag":0,"isFuture":false}]}]}]}
```

&#x20;

Can also use grep and awk to sum it across partitions.

```python
kafka-log-dirs --bootstrap-server localhost:9092 --describe \
    --topic-list people | grep -oP '(?<=size":)\d+' | awk '{ sum += $1 } END { print sum }'
```

### Consuming From a Specific Offset and Number of Messages <a href="#consumer-seeking" id="consumer-seeking"></a>

From within the broker container run the following to consume messages from the pets topic between offset 2 and 5

```python
kafka-console-consumer --bootstrap-server localhost:9092 --topic pets \
    --offset 2 --max-messages 3 --partition 0 --property print.offset=true
```

Output.

```python
Offset:2	{"name": "Toothless", "movie": "How To Train Your Dragon"}
Offset:3	{"name": "Willy", "movie": "Free Willy"}
Offset:4	{"name": "Babe", "movie": "Babe"}
Processed a total of 3 messages
```

### Deleting a Topic <a href="#delete-topic" id="delete-topic"></a>

Run the following to delete the people topic in broker container.

```python
kafka-topics --bootstrap-server localhost:9092 --delete --topic people
```

### Producing and Consuming Avro Messages <a href="#avro-pub-sub" id="avro-pub-sub"></a>

Through either installing Confluent platform or using the Confluent Schema Registry Docker image you can work with Avro enabled console producer and consumer tools. In this article I've been working with a Dockerized Kafka environment complete with the Confluent Schema Registry so I'll use that.\
First create a topic named dinosaurs from within the broker container.

```python
kafka-topics --bootstrap-server localhost:9092 --create --topic dinosaurs --replication-factor 1 --partitions 1
```

Then from within the schema-registry container create the following Avro Schema Definition File named dinos.avsc

```python
cat <<EOF >./dinos.avsc
{
  "type":"record",
  "namespace":"dinos",
  "name":"Dinosaur",
  "fields": [
    {
      "name":"name",
      "type":"string",
      "doc":"Name of Dinosaur"
    },
    {
      "name":"weight_low",
      "type":"int",
      "doc":"Weight lower bounds in metric tons"
    },
    {
      "name":"weight_high",
      "type":"int",
      "doc":"Weight upper bounds in metric tons"
    },
    {
      "name":"length_low",
      "type":"int",
      "doc":"Length lower bounds in meters"
    },
    {
      "name":"length_high",
      "type":"int",
      "doc":"Length upper bounds in meters"
    }
  ]
}
EOF
```

Now still within the schema-registry container produce some dinosaurs specifying the url of the Schema Registry (localhost).

```python
kafka-avro-console-producer --topic dinosaurs --broker-list broker:29092 \
  --property schema.registry.url="" \
  --property value.schema="$(< dinos.avsc)"
>{"name":"Ankylosaurus", "weight_low": 5, "weight_high": 8, "length_low": 6, "length_high": 8}
>{"name":"Carnotaurus", "weight_low": 1, "weight_high": 2, "length_low": 7, "length_high": 8}
>{"name":"Tyrannosaurus", "weight_low": 8, "weight_high": 14, "length_low": 10, "length_high": 12}
>{"name":"Triceratops", "weight_low": 10, "weight_high": 12, "length_low": 7, "length_high": 9}
```

_CTRL+C to exit producer_

Then run the following to consume the avro based messages.

```python
kafka-avro-console-consumer --bootstrap-server broker:29092 --topic dinosaurs --from-beginning \
  --property schema.registry.url=""
```

Output.

```python
{"name":"Ankylosaurus","weight_low":5,"weight_high":8,"length_low":6,"length_high":8}
{"name":"Carnotaurus","weight_low":1,"weight_high":2,"length_low":7,"length_high":8}
{"name":"Tyrannosaurus","weight_low":8,"weight_high":14,"length_low":10,"length_high":12}
{"name":"Triceratops","weight_low":10,"weight_high":12,"length_low":7,"length_high":9}
```

### Resources for Further Learning <a href="#learn-more" id="learn-more"></a>

* [Apache Kafka Crash Course for Python and Java Developers](https://www.udemy.com/course/apache-kafka-crash-course-for-java-and-python-developers/?referralCode=520CEA63E72E3AF1DDE7) is specifically designed to get developers knowledgable and productive across all major aspects of this powerful technology.
* The Coding Interface has written several [articles on Apache Kafka](https://thecodinginterface.com/blog/tag/kafka/) covering a varity of topics and use cases
