# Testing with embedded Kafka



```
// https://mvnrepository.com/artifact/org.springframework.kafka/spring-kafka-test
testImplementation 'org.springframework.kafka:spring-kafka-test:2.8.5'
```



There are several ways to provide system property:

* Provide a system property to map embedded broker addresses into `spring.kafka.bootstrap-servers` in the test class:

```java
static {
    System.setProperty(EmbeddedKafkaBroker.BROKER_LIST_PROPERTY, "spring.kafka.bootstrap-servers");
}
```

* Configure a property name on the `@EmbeddedKafka` annotation:

```java
@SpringBootTest
@EmbeddedKafka(topics = "someTopic", bootstrapServersProperty = "spring.kafka.bootstrap-servers")
class MyTest {

    @Autowired
    private KafkaConsumer consumer;

    @Autowired
    private KafkaProducer producer;
    // ...

}
```

* Use a placeholder in configuration properties:

properties.yaml

```yaml
spring:
  kafka:
    bootstrap-servers: "${spring.embedded.kafka.brokers}"
```
