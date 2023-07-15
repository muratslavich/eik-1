# High Availability

* High availability HA cluster
* fault tolerance - is a system’s ability to stay up despite taking hits.
* redundancy **-** is duplicating the components or instances and keeping extras on standby to take over in case the active instances go down. It is the fail-safe.
  * disk mirroring or RAID
  * redundant network connections
  * redundant nodes



#### Reasons for failures

* Software crashes
* Hardware failures
  * CPU and RAM overloaded
  * hard disk
  * nodes going down
  * network outages
* Human errors
* Planned downtime



If service run several instances, a few go offline, the system can work without going down entirely.

**Fail soft -** In the case of backend node failures, a few services of the app such as image upload, post likes etc. may stop working. However, the application as a whole will still be up.



**Microservices** loosely coupled and if some going down all other still work.&#x20;



### _active-passive HA mode_

An initial set of nodes are active, and a set of redundant nodes are passive, on standby. Active nodes get replaced by passive nodes, in case of failures.

![](<./../.gitbook/assets/image (27).png>)



### Get rid of single points of failure

Opposite the monolith. Redundant nodes help to have no single points of failure.



### Monitoring

* real time monitoring detect any bottlenecks or single points of failure
* self recover automation on the fly



### active-active HA mode

* no standby or passive instances
* having a number of similar replication
* nodes running the workload together

![](<./../.gitbook/assets/image (17).png>)



### Shared distributed memory

A **single state** across all the nodes in a cluster is achieved with the help of a shared distributed memory and a distributed coordination service like the _Zookeeper_.

![](<./../.gitbook/assets/image (31).png>)



### Geographical distribution

Geographical zones help with

* reduce latency
* avoid single point of failure
* natural disasters
* data centers workloads



