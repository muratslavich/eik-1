# Kafka consumer

### Consumer groups



* Kafka consumers are part of a consumer group.
* In a group, each consumer receives messages from a different subset of the partitions in the topic.
* Create a
* It's only
* **Consumers share the load in the group**
* No way to add more consumers than partitions, some of them will just be idle.
* It's a reason to create topics with a large number of partitions.

![](broken-reference)

![](<.gitbook/assets/image (4).png>)

* Single consumer in the group - get all messages from all partitions

![](broken-reference)

![](<.gitbook/assets/image (2).png>)

* Each consumer in the group gets messages only from his own subset of partitions

![](broken-reference)

![](<.gitbook/assets/image (3).png>)

* The same count of partitions, each will read from a single partition.
* More consumers than partitions, some consumers will be idle and get no messages.

![](broken-reference)

![](<.gitbook/assets/image (8).png>)

* If we want to application get all the messages in the topic, ensure the application has its own consumer group.

### Rebalance consumer's partitions



it happens when:

* add new consumer to the group
* consumer shuts down or crashes
* consumer leaves the group
* the topic is modified (added new partition)

Consumer starts consuming messages from partitions previously consumed by another consumer.

Rebalancing provides availability and scalability.

During a rebalance, consumers can't consume messages

**Group coordinator** - it's a broker that receives heartbeats from group members.

* as long as the consumer sending heartbeats at regular intervals, it's assumed to be alive.
* when consumer stops sending heartbeats the group coordinator considers it dead and
* consumer can notify GC that it is leaving, and GC will trigger a rebalance.

**Group leader** - first joined the group consumer.

**Joining the group or rebalancing**:

1. Consumer wants to join sends a JoinGroup request to the GC.
2. Group leader receives a list of all consumers in the group from the GC.
3. Leader decides partition assignment.
4. Leader sends the list of assignments to the GC.
5. GC sends assignments to all consumers.
