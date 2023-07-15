# The number of consumed messages

**`Consumer.poll()`** batch size can be configured by:

* max.poll.records (default 500) - the maximum number of records returned in a single call to poll()
* [**max.partition.fetch.bytes**](https://kafka.apache.org/documentation/#consumerconfigs\_max.partition.fetch.bytes) (default 1 048 576)
* [**fetch.max.wait.ms**](https://kafka.apache.org/documentation/#consumerconfigs\_fetch.max.wait.ms) - The maximum amount of time the server will block before answering the fetch request if there isn't sufficient data to immediately satisfy the requirement given by `fetch.min.bytes`.
* [**fetch.max.bytes**](https://kafka.apache.org/documentation/#brokerconfigs\_fetch.max.bytes) (default 52 428 800)
* [**fetch.min.bytes**](https://kafka.apache.org/documentation/#consumerconfigs\_fetch.min.bytes) (default 1 byte) - Minimum amount of data the server should return for a fetch request.

[https://kafka.apache.org/30/javadoc/org/apache/kafka/clients/consumer/ConsumerConfig.html](https://kafka.apache.org/30/javadoc/org/apache/kafka/clients/consumer/ConsumerConfig.html)

```
ConsumerConfig.FETCH_MIN_BYTES_CONFIG
ConsumerConfig.FETCH_MAX_BYTES_CONFIG
ConsumerConfig.MAX_PARTITION_FETCH_BYTES_CONFIG
ConsumerConfig.MAX_POLL_RECORDS_CONFIG
```

```java
@EmbeddedKafka(
        topics = ['test_transactions_topic', 'test_transactions_errors_topic'],
        brokerProperties = "log.dir=out/kafka"
)
@ActiveProfiles("test")
@SpringBootTest(
        classes = [Application],
        webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT,
        value = [
                "elastic.template.create.enabled: false",
                "kafka.cash-order.consumer.enabled: true",
                "spring.kafka.consumer.max-poll-records: 100",
                "spring.kafka.consumer.fetch-min-size: 3048576",
                "kafka.transactions.consumer.enabled: true"
        ]
)
@DirtiesContext(classMode = DirtiesContext.ClassMode.AFTER_CLASS)
class TransactionsListenerErrorHandlingTest { ... }
```
