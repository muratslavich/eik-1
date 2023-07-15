# Kafka producer

Produce message



1. Create ProducerRecord\[Topic, Partition, Key, Value]. Partition and Key are optional.
2. Serialize Key and Value to send over the network
3. Partitioner choose partition -
   1. If the partition is specified, the partitioner is skipped
   2. Partitioner choose partition based on key. Kafka's DefaultPartitioner uses a
   3. not providing a key - will result in
4. Add the records to a batch, that will also be sent to the same topic and partition
5. Request metadata from the controller(or any broker)??
   1. which brokers are alive
   2. where the leader for the partition?
6. Send message or batch (asynchronously) to the leader broker.
7. Waits for acknowledgment or not
   1. If the messages were written it will return RecordMetadata (it contains topic, partition, and offset).
   2. **If failed it will return an error**
8. A producer may retry sending after receives an error,

* All records with the
* Order is guaranteed in the partition, but not in the whole topic.
* Producer

\> Once a partition is selected, the producer knows which topic and partition the future records will go to.

Acknowledgment:

* **acks=0**
  * no guarantee can be made that the broker has received the message
  * retries configuration will not take effect
  * the given back offset would be -1
* **acks=1**
  * if the leader will fail right after acknowledge, but before followers replicate, the message will be lost
* **acks=all**
  * The message won't be lost.
  * Longest waiting.

### ![](<.gitbook/assets/image (14).png>)

### Producer configuration - [https://kafka.apache.org/documentation/#producerconfigs](https://kafka.apache.org/documentation/#producerconfigs)



* key.serializer
* value.serializer
* `acks=0`
* bootstrap.servers =
* retries = 0 .. Int.MaxValue
* batch.size

### Fire and forget



Send a message to the server and don't care about it further.

```
ProducerRecord<String, String> record = new ProducerRecord<>("Topic", "Key", "Value");
try {
    producer.send(record); 
} catch (Exception ex) {
    ...
}
```

The message will be placed in a buffer and will be sent to the broker in a separate thread.

Producer's send() method return `Future<RecordMetadata>` .

While we ignore send() response, we don't know whether the message was sent successfully or not. The **producer will retry sending messages automatically**. However, some messages will get lost.

While we ignore errors that occur while sending messages to the broker, we may still **catch an exception before sending the message**.

* SerializationException
* BufferExhaustedException
* TimeoutException

### Synchronous send



```
producer.send(record).get()
```

* We use get() to wait for a response.
* Throws an exception if the record is not sent successfully.
* Also throws an exception if there were

### Asynchronous send



If we wait for a reply after sending a huge batch of messages it will take time.

Usually, RecordMetadata is not required by the sending application, but **we need to know when we failed to send** a message completely so we can throw our own exception, log an error, or write the message to the errors store.

`org.apache.kafka.clients.producer.Callback`

```
producer.send(producerRecord, (recordMetadata, exception) -> {
      if (exception == null) {
          System.out.println("Record written to offset " +
                  recordMetadata.offset() + " timestamp " +
                  recordMetadata.timestamp());
      } else {
          System.err.println("An error occurred");
          exception.printStackTrace(System.err);
      }
});
```

### Errors



**Retriable errors** can be solved by sending the message again. The producer can retry it automatically, the **application will get a retriable exception only when the number of retries was exhausted** and the error was not resolved.

* Connection errors
* "no leader" error

**Nonretriable errors** that can not be resolved by retrying. Producer returns exception immediately.

* message size too large

We will want to focus our efforts on **handling nonretriable errors or cases where retries were exhausted**.

### Ordering guarantees



Kafka guarantees order within a partition.

But if `retries` parameter isn't zero and `max.in.flights.requests.per.session` is more than one, means that it's possible that the broker will fail to write the first batch of messages, succeed to write the second (which was already in-flight), and then retry the first batch.

So if ordering is critical use: `in.flight.requests.per.session=1`
