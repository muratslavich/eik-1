# Kafka overview

## Dictionary



* Broker
  * receives messages from producers
  * assigns offset
  * **commits message to the disk**
  * services consumers, responding to fetch request
  * part of cluster
* Controller
  * automatically elected living broker
  * assigns partitions to brokers
  * monitoring for broker failures
  * **responsible for leader election**
* Cluster
  * set of brokers
* Producer
  * create new messages, publishers
* Consumer
  * read messages
  * controls his offset
* Consumers group
  * one or more consumers that work together to consume a topic
  * assure that each partition is
  * **way to horizontal scale consuming topic**
  * if a consumer fails, the remaining members of a group will rebalance.
* Group coordinator
* Topic
  * name of the message category
  * retention - the period after which all messages will be discarded to free up space
* Partition
  * each topic can have multiple partitions
  * single commit log
  * the way that Kafka provides redundancy and scalability
  * each partition can be hosted on different brokers
  * **way to horizontal scale producing topic**
* Replica
  * each partition can have multiple replicas
  * the
  * in-sync replicas - those replicas who completely sync with the leader, eligible to be elected as a new leader.
* (Replica) Leader
  * the broker who owns this partition, other brokers containing replicas of partition are followers
  * each partition has a single leader replica
  * **all produce and consume requests go through the leader**
  * preferred leader - the replica that was a leader when the topic was created.
* Offset
  * unique integer index in a given partition
  * **high watermark -**
* Connectors
  * allows connecting topics to existing bd, might capture every change to a table.
* Streams
* Zookeeper
  * controller election
  * cluster membership - keeps a list of brokers
  * topic configuration - list of topics, number of partition, replicas, preferred leader node, and other topics overrides
  * access control list- for all topics, who or what is allowed to read/write
  * quotas - how much data each client is allowed to read/write

## Algorithms



* Write message
* Read message
* Partition assignment/reassignment
* Rebalance consumer's partitions
* Controller election
* Leader election
* Leader rebalancing
* Zookeeper consensus
