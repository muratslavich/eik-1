# Kafka cluster scheme

![](<.gitbook/assets/image (11).png>)

### 3 data centers

![](<.gitbook/assets/image (12).png>)

* rack.id=\<location>
* replica.selector.class= \<ReplicaSelector impl>
* Out of the box:
  * LeaderSelector (default)
  * RackAwareReplicaSelector



### 2 data centers

![](<.gitbook/assets/image (13).png>)

* If dc1 goes down zookeeper wouldn't keep the quorum consensus.
* Outage.



![](<.gitbook/assets/image (15).png>)

* Hierarchical zookeeper quorum.
* Brokers configured to talk with local zk's.
* Availability over consistency.
* Clients lose visibility of partition leaders in other DC
* Production either partially continues or blocks, depending on replication settings
* Manual intervention required to resume processing

<figure><img src=".gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

![](<.gitbook/assets/image (5).png>)

* ZooKeeper behavior is same as 3DC setup.
* Replication tradeoffs are the same as 2DC setup.



Mirroring clusters

![](.gitbook/assets/image.png)

