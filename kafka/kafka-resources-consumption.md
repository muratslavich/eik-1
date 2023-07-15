# Kafka resources consumption

**CPUs**\
Most Kafka deployments tend to be rather light on CPU requirements. As such, the exact processor setup matters less than the other resources. Note that if SSL is enabled, the CPU requirements can be significantly higher (the exact details depend on the CPU type and JVM implementation).

You should choose a modern processor with multiple cores. Common clusters utilize 24 core machines.

If you need to choose between faster CPUs or more cores, choose more cores. The extra concurrency that multiple cores offers will far outweigh a slightly faster clock speed.





_**How to compute your throughput**_\
It might also be helpful to compute the throughput. For example, if you have 800 messages per second, of 500 bytes each then your throughput is `800*500/(1024*1024) = ~0.4MB/s`. Now if your topic is partitioned and you have 3 brokers up and running with 3 replicas that would lead to `0.4/3*3=0.4MB/s` per broker.





**ZooKeeper** uses the JVM heap, and 4GB RAM is typically sufficient. Too small of a heap will result in high CPU due to constant garbage collection while too large heap may result in long garbage collection pauses and loss of connectivity within the ZooKeeper cluster.





**Kafka brokers** use both the JVM heap and the OS page cache. The JVM heap is used for replication of partitions between brokers and for log compaction. Replication requires 1MB (default replica.max.fetch.size) for each partition on the broker. In Apache Kafka 0.10.1 (Confluent Platform 3.1), we added a new configuration (replica.fetch.response.max.bytes) that limits the total RAM used for replication to 10MB, to avoid memory and garbage collection issues when the number of partitions on a broker is high. For log compaction, calculating the required memory is more complicated and we recommend referring to the Kafka documentation if you are using this feature. For small to medium-sized deployments, 4GB heap size is usually sufficient. In addition, it is highly recommended that consumers always read from memory, i.e. from data that was written to Kafka and is still stored in the OS page cache. The amount of memory this requires depends on the rate at this data is written and how far behind you expect consumers to get. If you write 20GB per hour per broker and you allow brokers to fall 3 hours behind in normal scenario, you will want to reserve 60GB to the OS page cache. In cases where consumers are forced to read from disk, performance will drop significantly

**Kafka Connect** itself does not use much memory, but some connectors buffer data internally for efficiency. If you run multiple connectors that use buffering, you will want to increase the JVM heap size to 1GB or higher.

**Consumers** use at least 2MB per consumer and up to 64MB in cases of large responses from brokers (typical for bursty traffic). **Producers** will have a buffer of 64MB each. Start by allocating 1GB RAM and add 64MB for each producer and 16MB for each consumer planned.
