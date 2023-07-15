# Vertical and Horizontal Scaling

**Vertical scaling** - Is adding more resources (adding memory). The simplest one. Do not have to touch the code or make any complex distributed system configurations. There is always a **risk of them going down** and the entire website going offline.

* I/O capacity - adding more hard drives in RAID or change to SSD. It can be bottleneck in database servers.
* Memory upgrade - more memory for applications and system cache, that means reduce I/O operations.
* CPU upgrade - The more CPUs and cores , the more processes that can be executing. But concurrent data has to be locked between threads, and **Lock Contention can be bottleneck** (when application spend most of its time waiting for locks to be released).
* Network upgrade - if server stream a lot of content.

**Horizontal** - adding more hardware to the pool. This increases the computational power of the system as a whole. Add more servers/data centers to have the ability to **dynamically scale** in real-time as the traffic increases and decreases over a period of time. **High availability** of the system.

**Cloud computing.**\
The biggest reason why _cloud computing_ become so popular in the industry is the ability to scale up and down dynamically. The ability to use and pay only for the resources required by the website became a trend for obvious reasons.

Distributed environment requires **administrative, monitoring, logging** efforts.



### Code maintaining

In distributed environment application has to be **stateless**. No static/state instances in classes that hold application data. Use key-value store to hold data. That's why functional programming became so popular - the function doesn't have a state.



### Functional partitioning

Also one of the first steps to scaling might be moving different parts of the system to separate physical servers and scale them vertically.

* for example split database, cache and application
* split the monolithic application into a set of functional parts and host it independently



### Database bottleneck

When the workload runs on multiple nodes, and it has the ability to scale horizontally, the database is a single monolith that has to handle all requests. Way to scale database:

{% embed url="https://medium.com/swlh/5-database-scaling-solutions-you-need-to-know-e307570efb72" %}

* cache queries
* indexes
* replication
* partitioning
* sharding



Also, it can be other problems for scaling:

* appliaction architecture
* not using cacheng wisely
* load balancer inefficient configuration
* business logic in the database
* not right database
* code-level problems
