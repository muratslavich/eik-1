# Spring Kafka

{% embed url="https://docs.spring.io/spring-kafka/docs/current/reference/html/" %}

{% tabs %}
{% tab title="Consumer" %}


```java
@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

    @Bean
    public NewTopic topic() {
        return TopicBuilder.name("topic1")
                .partitions(10)
                .replicas(1)
                .build();
    }

    @KafkaListener(id = "myId", topics = "topic1")
    public void listen(String in) {
        System.out.println(in);
    }

}
```
{% endtab %}

{% tab title="Producer" %}
```java
@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

    @Bean
    public NewTopic topic() {
        return TopicBuilder.name("topic1")
                .partitions(10)
                .replicas(1)
                .build();
    }

    @Bean
    public ApplicationRunner runner(KafkaTemplate<String, String> template) {
        return args -> {
            template.send("topic1", "test");
        };
    }

}
```
{% endtab %}

{% tab title="Gradle" %}
```groovy
compile 'org.springframework.kafka:spring-kafka:2.8.5'
```
{% endtab %}

{% tab title="Properties" %}
```
spring:
   kafka:
     consumer:
        bootstrap-servers: localhost:9092
        group-id: group_id
        auto-offset-reset: earliest
        key-deserializer: org.apache.kafka.common.serialization.StringDeserializer
        value-deserializer: org.apache.kafka.common.serialization.StringDeserializer
     producer:
        bootstrap-servers: localhost:9092
        key-serializer: org.apache.kafka.common.serialization.StringSerializer
        value-serializer: org.apache.kafka.common.serialization.StringSerializer
```
{% endtab %}

{% tab title="JavaConfig" %}


```java
@Configuration
@EnableKafka
public class Config {

    @Bean
    ConcurrentKafkaListenerContainerFactory<Integer, String> kafkaListenerContainerFactory(
        ConsumerFactory<Integer, String> consumerFactory
    ) {
        ConcurrentKafkaListenerContainerFactory<Integer, String> factory = new ConcurrentKafkaListenerContainerFactory<>();
        factory.setConsumerFactory(consumerFactory);
        factory.setBatchListener(true);  // < for batch receiving
        return factory;
    }

    @Bean
    public ConsumerFactory<Integer, String> consumerFactory() {
        return new DefaultKafkaConsumerFactory<>(consumerProps());
    }

    private Map<String, Object> consumerProps() {
        Map<String, Object> props = new HashMap<>();
        props.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
        props.put(ConsumerConfig.GROUP_ID_CONFIG, "group");
        props.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, IntegerDeserializer.class);
        props.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class);
        props.put(ConsumerConfig.AUTO_OFFSET_RESET_CONFIG, "earliest");
        // ...
        return props;
    }
    
    
}
```

```java
    @Bean
    public ProducerFactory<Integer, String> producerFactory() {
        return new DefaultKafkaProducerFactory<>(senderProps());
    }

    private Map<String, Object> senderProps() {
        Map<String, Object> props = new HashMap<>();
        props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
        props.put(ProducerConfig.LINGER_MS_CONFIG, 10);
        props.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, IntegerSerializer.class);
        props.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, StringSerializer.class);
        //...
        return props;
    }

    @Bean
    public KafkaTemplate<Integer, String> kafkaTemplate(ProducerFactory<Integer, String> producerFactory) {
        return new KafkaTemplate<Integer, String>(producerFactory);
    }

```
{% endtab %}
{% endtabs %}



### Configuring topics

KafkaAdmin - automatically add topics to the broker.

<details>

<summary>topics</summary>

```java
@Bean
public KafkaAdmin admin() {
    Map<String, Object> configs = new HashMap<>();
    configs.put(AdminClientConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
    return new KafkaAdmin(configs);
}

@Bean
public NewTopic topic1() {
    return TopicBuilder.name("thing1")
            .partitions(10)
            .replicas(3)
            .compact()
            .build();
}

@Bean
public NewTopic topic4() {
    return TopicBuilder.name("defaultBoth")
            .build();
}

@Bean
public NewTopic topic2() {
    return TopicBuilder.name("thing2")
            .partitions(10)
            .replicas(3)
            .config(TopicConfig.COMPRESSION_TYPE_CONFIG, "zstd")
            .build();
}

@Bean
public NewTopic topic3() {
    return TopicBuilder.name("thing3")
            .assignReplicas(0, Arrays.asList(0, 1))
            .assignReplicas(1, Arrays.asList(1, 2))
            .assignReplicas(2, Arrays.asList(2, 0))
            .config(TopicConfig.COMPRESSION_TYPE_CONFIG, "zstd")
            .build();
}

@Bean
public KafkaAdmin.NewTopics topics456() {
    return new NewTopics(
            TopicBuilder.name("defaultBoth")
                .build(),
            TopicBuilder.name("defaultPart")
                .replicas(1)
                .build(),
            TopicBuilder.name("defaultRepl")
                .partitions(3)
                .build());
}
```

</details>



### Sending message

<details>

<summary>KafkaTemplate -  wraps a producer</summary>

```java
ListenableFuture<SendResult<K, V>> sendDefault(V data);
ListenableFuture<SendResult<K, V>> sendDefault(K key, V data);
ListenableFuture<SendResult<K, V>> sendDefault(Integer partition, K key, V data);
ListenableFuture<SendResult<K, V>> sendDefault(Integer partition, Long timestamp, K key, V data);
ListenableFuture<SendResult<K, V>> send(String topic, V data);
ListenableFuture<SendResult<K, V>> send(String topic, K key, V data);
ListenableFuture<SendResult<K, V>> send(String topic, Integer partition, K key, V data);
ListenableFuture<SendResult<K, V>> send(String topic, Integer partition, Long timestamp, K key, V data);
ListenableFuture<SendResult<K, V>> send(ProducerRecord<K, V> record);
ListenableFuture<SendResult<K, V>> send(Message<?> message);

Map<MetricName, ? extends Metric> metrics();
List<PartitionInfo> partitionsFor(String topic);

<T> T execute(ProducerCallback<K, V, T> callback);

// Flush the producer.
void flush();

interface ProducerCallback<K, V, T> {
    T doInKafka(Producer<K, V> producer);
}
```

</details>

To use the template, you can configure a **`ProducerFactory`** and provide it in the template’s constructor.

Result callback

```java
ListenableFuture<SendResult<Integer, String>> future = template.send("topic", 1, "thing");
future.addCallback(result -> {
        ...
    }, (KafkaFailureCallback<Integer, String>) ex -> {
            ProducerRecord<Integer, String> failed = ex.getFailedProducerRecord();
            ...
    });
```



**`RoutingKafkaTemplate`** - use to select producer at runtime, based on destination topic name.

**`DefaultKafkaProducerFactory`** creates a singleton producer used by all clients.

producerPerThread=false by default.

**`ReplyingKafkaTemplate`** has two additional methods

```java
RequestReplyFuture<K, V, R> sendAndReceive(ProducerRecord<K, V> record);
RequestReplyFuture<K, V, R> sendAndReceive(ProducerRecord<K, V> record, Duration replyTimeout);
```

```java
 @KafkaListener(id="server", topics = "kRequests")
 @SendTo // use default replyTo expression
 public String listen(String in) {
      System.out.println("Server received: " + in);
      return in.toUpperCase();
 }
```

**`AggregatingReplyingKafkaTemplate`** - For cases where multiple receivers of a single message return a reply



### Receiving messages

Configure **`MessageListenerContainer`** and provide a `MessageListener` or **`@KafkaListener`**

* `KafkaMessageListenerContainer`- receives all message from all topics or partitions on a single thread.
* `ConcurrentMessageListenerContainer` delegates to one or more `KafkaMessageListenerContainer` instances to provide multi-threaded consumption. For example, `container.setConcurrency(3)` creates three `KafkaMessageListenerContainer` instances.

If the `concurrency` is greater than the number of `TopicPartitions`, the `concurrency` is adjusted down such that each container gets one partition.

<details>

<summary>MessageListeners</summary>

```java
процессинг единичного сообщения из консамера poll()
auto-commit или container-managed коммит
public interface MessageListener<K, V> { 
    void onMessage(ConsumerRecord<K, V> data);
}

процессинг единичного сообщения из консамера poll()
ручной коммит
public interface AcknowledgingMessageListener<K, V> { 
    void onMessage(ConsumerRecord<K, V> data, Acknowledgment acknowledgment);
}

Доступ к объекту Consumer<>
public interface ConsumerAwareMessageListener<K, V> extends MessageListener<K, V> { 
    void onMessage(ConsumerRecord<K, V> data, Consumer<?, ?> consumer);
}

Доступ к объекту Consumer<> и ручной коммит
public interface AcknowledgingConsumerAwareMessageListener<K, V> extends MessageListener<K, V> { 
    void onMessage(ConsumerRecord<K, V> data, Acknowledgment acknowledgment, Consumer<?, ?> consumer);
}

процессинг всех записей из poll()
auto-commit or container-managed commit
public interface BatchMessageListener<K, V> { 
    void onMessage(List<ConsumerRecord<K, V>> data);
}

процессинг всех записей из poll()
manual commit
public interface BatchAcknowledgingMessageListener<K, V> { 
    void onMessage(List<ConsumerRecord<K, V>> data, Acknowledgment acknowledgment);
}

Батч, Consumer<?, ?> и auto-commit
public interface BatchConsumerAwareMessageListener<K, V> extends BatchMessageListener<K, V> { 
    void onMessage(List<ConsumerRecord<K, V>> data, Consumer<?, ?> consumer);
}

Батч, Consumer<?, ?> и manual-commit
public interface BatchAcknowledgingConsumerAwareMessageListener<K, V> extends BatchMessageListener<K, V> { 
    void onMessage(List<ConsumerRecord<K, V>> data, Acknowledgment acknowledgment, Consumer<?, ?> consumer);
}
```

</details>



#### Committing offsets

default `AckMode` is `BATCH`

The `MessageListener` is called for each record.

`enable.auto.commit: false`&#x20;

* **RECORD** commit offset after listener processing record
* **BATCH**  after all records from poll() have been processed
* **TIME**  after all records or after **`ackTime`**
* **COUNT** after all records or after **`ackCount`** records
* **COUNT\_TIME** all or `ackTime` or `ackCount`
* **MANUAL** listener responsible for acknowledge() BATCH semantic are applied
* **MANUAL\_IMEDIATE** commit immediately&#x20;



Because the listener container has it’s own mechanism for committing offsets, it prefers the Kafka `ConsumerConfig.ENABLE_AUTO_COMMIT_CONFIG` to be `false`



**`syncCommits`** container property default true



Normally, when using `AckMode.MANUAL` or `AckMode.MANUAL_IMMEDIATE`, the acknowledgments must be acknowledged in order, because Kafka does not maintain state for each record, only a committed offset for each group/partition.

```java
@KafkaListener(id = "cat", topics = "myTopic",
          containerFactory = "kafkaManualAckListenerContainerFactory")
public void listen(String data, Acknowledgment ack) {
    ...
    ack.acknowledge();
}
```



the possibility of **duplicate deliveries** after a failure



Listener starting offset

```java
@KafkaListener(id = "thing3", topicPartitions =
        { @TopicPartition(topic = "topic1", partitions = { "0", "1" },
             partitionOffsets = @PartitionOffset(partition = "*", initialOffset = "0"))
        })
public void listen(ConsumerRecord<?, ?> record) {
    ...
}
```



Receive message headers

```java
@KafkaListener(id = "qux", topicPattern = "myTopic1")
public void listen(@Payload String foo,
        @Header(name = KafkaHeaders.RECEIVED_MESSAGE_KEY, required = false) Integer key,
        @Header(KafkaHeaders.RECEIVED_PARTITION_ID) int partition,
        @Header(KafkaHeaders.RECEIVED_TOPIC) String topic,
        @Header(KafkaHeaders.RECEIVED_TIMESTAMP) long ts
        ) {
    ...
}

// or ConsumerRecordMetadata

@KafkaListener(...)
public void listen(String str, ConsumerRecordMetadata meta) {
    ...
}
```



Receive batch

```java
@KafkaListener(id = "listCRs", topics = "myTopic", containerFactory = "batchFactory")
public void listen(List<ConsumerRecord<Integer, String>> list) {
    ...
}

@KafkaListener(id = "listCRsAck", topics = "myTopic", containerFactory = "batchFactory")
public void listen(List<ConsumerRecord<Integer, String>> list, Acknowledgment ack) {
    ...
}

@KafkaListener(id = "list", topics = "myTopic", containerFactory = "batchFactory")
public void listen(ConsumerRecords<?, ?> records) {
    ...
}
```

