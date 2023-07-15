# Confluent kafka configurations

{% embed url="https://kafka.apache.org/documentation#topicconfigs" %}

{% embed url="https://docs.confluent.io/platform/current/installation/docker/config-reference.html#docker-configuration-parameters" %}

For the Kafka (`cp-kafka`) image, convert the `kafka.properties` file variables as below and use them as environment variables:

* Prefix with `KAFKA_`.
* Convert to upper-case.
* Replace a period (`.`) with a single underscore (`_`).
* Replace a dash (`-`) with double underscores (`__`).
* Replace an underscore (`_`) with triple underscores (`___`).

For example, run the following commands to set `broker.id`, `advertised.listeners`, `zookeeper.connect`, and `offsets.topic.replication.factor`:

```
docker run -d \
    --net=host \
    --name=kafka \
    -e KAFKA_ZOOKEEPER_CONNECT=localhost:32181 \
    -e KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://localhost:29092 \
    -e KAFKA_BROKER_ID=2 \
    -e KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR=1 \
    confluentinc/cp-kafka:7.1.1
```
