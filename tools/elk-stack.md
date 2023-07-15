---
description: Logstash-elasticsearch-kibana
layout: editorial
---

# ELK stack



![](<./.gitbook/assets/image (37).png>)

![](<./.gitbook/assets/image (58).png>)

### **logstash**&#x20;

* broker - получает данные из разных источников, процессит и отправляет на хранение
* indexer - парсит лог по различным тэгам

### elasticsearch

* хранилище логов
* 3 master nodes
* 4 bumper data nodes for processing recent nodes
* 6 data archive nodes (3 warm - 3 hot)
* 1 client search node

Data nodes hold the shards that contain indexed documents and handle search and aggregation operations.

Bumper nodes hold recent 5-7 days old messages

Archive nodes have less computing resources and hold archived messages

Search node are used by Kibana for connections, holds no data

### Logs flow

* logs created in application
* using libraries (docker log-driver) send it to Logstash broker using tcp endpoint
* for log files (ex. system logs) we use Filebit - it listen certain file for changes, parse and send it
* In Logstash broker config we create endpoints and ports for different logs types &#x20;
* Logstash broker tags messages by metadata: endpoint port, unique name, group
* Logstash broker push messages to different buffers (kafka/redis) . It's vital when Logstash **indexers cannot handle loads**.
* Logstash indexer pull log messages and parse them based on specific tags
* Logstash indexer decide which Elasticsearch bumper will go onto. (ex. system and app logs)
* Elasticsearch store logs in indexes. ex applications can have own indexes.
* Elasticsearch master do delete relocate or optimization of indexes.

### Kafka weight

* Logstash process and filter logs - it can be bottleneck
* Elasticsearch needs to store logs in indexes - it also can be bottleneck

It can be smoothed by scaling Logstash/Elasticsearch cluster.

Or adding Redis caching in the middle of database access path

Or adding Kafka processing

&#x20;

![](<./.gitbook/assets/image (52).png>)

