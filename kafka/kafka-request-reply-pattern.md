# Kafka Request-Reply pattern

{% embed url="https://habr.com/ru/post/476156/" %}

Синхронный обмен сообщениями с помощью асинхронного канала

![](<.gitbook/assets/image (6).png>)

Паттерн [**Return Address (Адрес Возврата)**](https://www.enterpriseintegrationpatterns.com/patterns/messaging/ReturnAddress.html) дополняет паттерн [**Request-Reply**](https://www.enterpriseintegrationpatterns.com/patterns/messaging/RequestReply.html) механизмом указания запрашивающей стороной адреса, на который должен быть отправлен ответ:

![](<.gitbook/assets/image (10).png>)

### Requestor

```java
@Bean
 public ReplyingKafkaTemplate<String, Request, Reply> replyKafkaTemplate(
     ProducerFactory<String, Request> pf,
     KafkaMessageListenerContainer<String, Reply> lc
 ) {
     return new ReplyingKafkaTemplate<>(pf, lc);
 }
```

Конифгурация **`ProducerFactory`** Запроса, **`ConsumerFactory`** Ответа и **`MessageListenerContainer`**

```java
@Bean
 public ProducerFactory<String, Request> requestProducerFactory() {
     return new DefaultKafkaProducerFactory<> (producerConfigs());
 }

 @Bean
 public ConsumerFactory<String, Reply> replyConsumerFactory() {
     return new DefaultKafkaConsumerFactory<> (
         consumerConfigs(), 
         new StringDeserializer(),
         new JsonSerializer<Reply> ()
     );
 }

 @Bean
 public KafkaMessageListenerContainer <String, Reply> replyListenerContainer() {
     ContainerProperties containerProperties = new ContainerProperties(replyTopic);
     return new KafkaMessageListenerContainer<> (replyConsumerFactory(), containerProperties);
 }
```

**requestReplyKafkaTemplate** заботится о генерации и установке заголовка **KafkaHeaders.CORRELATION\_ID**, но мы должны явно задать заголовок **KafkaHeaders.REPLY\_TOPIC** для запроса

```java
RequestReplyFuture<String, Request, Reply> replyFuture = template.sendAndReceive(record);
SendResult<String, Reply> sendResult = replyFuture.getSendFuture().get(10, TimeUnit.SECONDS);
```

### Replier

На стороне сервера обычный **KafkaListener**, прослушивающий топик для запроса, дополнительно декорирован аннотацией **@SendTo**, чтобы предоставить сообщение-ответ.

Объект, возвращаемый методом слушателя, автоматически оборачивается (wrapped) в ответное сообщение, добавляется **CORRELATION\_ID** и ответ публикуется в топике, указанном в заголовке **REPLY\_TOPIC**.

```java
 @Bean
 public KafkaListenerContainerFactory <ConcurrentMessageListenerContainer<String, Request>> requestListenerContainerFactory() {
     ConcurrentKafkaListenerContainerFactory < String, Request > factory =
         new ConcurrentKafkaListenerContainerFactory<>();
     factory.setConsumerFactory(requestConsumerFactory());
     factory.setReplyTemplate(replyTemplate());
     return factory;
 }
 
 @KafkaListener(topics = "${kafka.topic.car.request}", containerFactory = "requestListenerContainerFactory")
 @SendTo()
 public Reply receive(Request request) {
     Reply reply = ...;
     return reply;
 }
```
