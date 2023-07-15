# Kafka cluster migration algorithm

## 1-2-3 -> 4-5-6

1. Add new brokers to cluster
   1. start brokers
   2. verify that they are part of cluster
   3. **NOTE:**
2. Add new cluster to the API's configuration
3. Increase replication factor
   1. get all topics
   2. create partition reassignment json file
   3. generate -> execute -> verify

```
usr/bin/kafka-reassign-partitions.sh --zookeeper localhost:2181 --topics-to-move-json-file replica-info.json --broker-list “1,2,4,5” --generate

// 50 mb/s
usr/bin/kafka-reassign-partitions.sh --zookeeper localhost:2181 --reassignment-json-file expand-cluster-reassignment.json --throttle 50000000 --execute

usr/bin/kafka-reassign-partitions.sh --zookeeper localhost:2181 --reassignment-json-file expand-cluster-reassignment.json --verify
```

1. Decommission old brokers
   1. shift url to new brokers
   2. shutdown old brokers
2. Decrease replication factor to its initial value
   1. repeat commands with new json file

```
cat replica-info.json
{  
   "version":1,
   "Partitions":[  
      {  
         "topic":"foo1",
         "partition":0,
         "replicas":[4, 5]
      },
      {  
         "topic":"foo1",
         "partition":1,
         "replicas":[4, 5]
      },
      {  
         "topic":"foo2",
         "partition":0,
         "replicas":[4, 5]
      },
      {  
         "topic":"foo2",
         "partition":1,
         "replicas":[4, 5]
      }
   ]
}

$ bin/kafka-reassign-partitions.sh --zookeeper localhost:2181 --topics-to-move-json-file replica-info.json --broker-list “1,2,4,5” --generate

$ bin/kafka-reassign-partitions.sh --zookeeper localhost:2181 --reassignment-json-file expand-cluster-reassignment.json --execute

$ bin/kafka-reassign-partitions.sh --zookeeper localhost:2181 --reassignment-json-file expand-cluster-reassignment.json --verify
```

Migrate zookeeper then

1. add zk node
   1. verify
2. do rolling restart of all the other instances with updated configuration file. Restart the leader last
3. decommission any of old
   1. shut down instance
   2. Remove instance N entry from the configuration file of instances and do a rolling restart. (Restart the leader last)
4. Repeat



I think, the process of adding 1 zk and removing 1zk is fine, but that would also require to update kafka broker config zookeeper.connect to point to new zk nodes, otherwise kafka brokers will not be able to talk to zookeeper.

I am trying below approach, and so far its working fine for me:

* Add new zk nodes(4,5,6) at a time.
* For ZK\[1..6] in rolling manner - Update the zoo.cfg file and restart zookeeper service. \[To make 4,5,6 to join cluster]
* For Kafka brokers, in rolling manner - Update zookeeper.connect config and restart Kafka service. \[To let Kafka know about new zk nodes]
* Remove old ZK node(1,2,3), and update ZK\[4,5,6] configs, restart in rolling manner.
* For Kafka brokers, in rolling manner — Update zookeeper.connect config and restart Kafka service. \[To let Kafka update to point to new ZK nodes only, and remove old ZK nodes from config]





\----------------------



```
1. добавить новые ноды в инвентарник
[kafka_broker_business]
corpint4 kafka_broker_id=1001
corpint5 kafka_broker_id=1002
corpint7 kafka_broker_id=1004
corpint8    kafka_broker_id=1005
corpint9    kafka_broker_id=1006
corpint11   kafka_broker_id=1007
corpint12   kafka_broker_id=1008

2. запустить новые ноды
ansible-playbook -i ./integration/inv-kafka-business kafka-business.yml -t kafka --limit '!corpint4:!corpint5:!corpint7'

3. проверить количество брокеров `echo dump | nc corpint8 2181`

4. повысить replication factor на новые ноды и сделать reassign
5. дождаться конца реассайна

6. добавить апишкам урлы на новых брокеров
bootstrap-servers:
    - 'corpint4:9092,corpint5:9092,corpint7:9092'
    - 'corpint8:9092,corpint9:9092,corpint11:9092,corpint12:9092'
перезапустить

7. выключить старые брокеры по очереди
8. уменьшить фактор репликации - реассайн

9. удалить в апишках страрые урлы
перезапустить

```

\------------------------



```
docker run --rm -it --dns-search=moscow.alfaintra.net infra.binary.alfabank.ru/cp-kafka:5.3.1 usr/bin/kafka-topics --zookeeper corpint4:2181 --list > ./reassignment/topics.json

docker run --rm -it --dns-search=moscow.alfaintra.net -v "$(pwd)"/reassignment:/app infra.binary.alfabank.ru/cp-kafka:5.3.1 usr/bin/kafka-reassign-partitions --zookeeper corpint4:2181 --topics-to-move-json-file /app/topics.json --broker-list "1001,1002,1004" --generate > ./reassignment/reassignments.json

docker run --rm -it --dns-search=moscow.alfaintra.net -v "$(pwd)"/reassignment:/app infra.binary.alfabank.ru/cp-kafka:5.3.1 usr/bin/kafka-reassign-partitions --zookeeper corpint4:2181 --reassignment-json-file /app/reassignment-new.json --throttle 50000000 --execute

docker run --rm -it --dns-search=moscow.alfaintra.net -v "$(pwd)"/reassignment:/app infra.binary.alfabank.ru/cp-kafka:5.3.1 usr/bin/kafka-reassign-partitions --zookeeper corpint4:2181 --reassignment-json-file /app/reassignment-new.json --verify




org.apache.zookeeper.KeeperException$NoAuthException: KeeperErrorCode = NoAuth for /config/topics/SIMPLE
	at org.apache.zookeeper.KeeperException.create(KeeperException.java:116)
	at org.apache.zookeeper.KeeperException.create(KeeperException.java:54)
	at kafka.zookeeper.AsyncResponse.maybeThrow(ZooKeeperClient.scala:560)
	at kafka.zk.KafkaZkClient.setOrCreateEntityConfigs(KafkaZkClient.scala:368)
	at kafka.zk.AdminZkClient.changeEntityConfig(AdminZkClient.scala:378)
	at kafka.zk.AdminZkClient.changeTopicConfig(AdminZkClient.scala:338)

http://support-it.huawei.com/docs/en-us/fusioninsight-all/maintenance-guide/en-us_topic_0222551464.html



$ docker exec -it kafka_um usr/bin/kafka-topics --zookeeper localhost:5151 --list

Error: JMX connector server communication error: service:jmx:rmi://corpint8.moscow.alfaintra.net:9988
sun.management.AgentConfigurationError: java.rmi.server.ExportException: Port already in use: 9988; nested exception is:
	java.net.BindException: Address already in use (Bind failed)
	at sun.management.jmxremote.ConnectorBootstrap.exportMBeanServer(ConnectorBootstrap.java:800)
	at sun.management.jmxremote.ConnectorBootstrap.startRemoteConnectorServer(ConnectorBootstrap.java:468)
	at sun.management.Agent.startAgent(Agent.java:262)
	at sun.management.Agent.startAgent(Agent.java:452)

```

\----------------------------



```
### Zookeeper migration

1. Добавить новые ноды zk - corpint8, corpint9, corpint11

[zookeeper_business]
corpint4 zookeeper_id=1
corpint5 zookeeper_id=2
corpint7 zookeeper_id=4
corpint10    zookeeper_id=7
corpint11    zookeeper_id=5
corpint12    zookeeper_id=6

ansible-playbook -i ./integration/inv-kafka-business kafka-business.yml -t zookeeper --limit '!corpint4:!corpint5:!corpint7'

2. Пересоздать zk ноды 1,2,4 по очереди, чтобы обновить у них zoo.cfg и присоеденить новые ноды к кластеру

ansible-playbook -i ./integration/inv-kafka-business kafka-business.yml -t zookeeper --limit 'corpint4'
ansible-playbook -i ./integration/inv-kafka-business kafka-business.yml -t zookeeper --limit 'corpint5'
ansible-playbook -i ./integration/inv-kafka-business kafka-business.yml -t zookeeper --limit 'corpint7'

--- cat /etc/zookeeper/zoo.cfg
# Server list for clusters of 3 or more hosts
server.1=corpint4:2888:3888
server.2=corpint5:2888:3888
server.4=corpint7:2888:3888
server.5=corpint9:2888:3888
server.6=corpint11:2888:3888
server.7=corpint8:2888:3888

3. Проверить zk кластер `echo stat | nc corpint4 2181`

4. Пересоздать по очереди всех кафка брокеров с новым конфигом zookeeper.connect
Прописывается в env `-e KAFKA_ZOOKEEPER_CONNECT={{ kafka_zk_connect }}`

ansible-playbook -i ./integration/inv-kafka-business kafka-business.yml -t kafka --limit 'corpint4'
ansible-playbook -i ./integration/inv-kafka-business kafka-business.yml -t kafka --limit 'corpint5'
ansible-playbook -i ./integration/inv-kafka-business kafka-business.yml -t kafka --limit 'corpint7'

проверять между брокерами, что они стартанули и начали отвечать

Проверить cat /etc/systemd/system/kafka.service
-e KAFKA_ZOOKEEPER_CONNECT=corpint4:2181,corpint5:2181,corpint7:2181,corpint8:2181,corpint9:2181,corpint11:2181 \

5. Остановить старые ноды по очереди
sudo systemctl start zookeeper.service
sudo rm /etc/systemd/system/zookeeper.service

6. Убрать старые ноды из инвентарника
[zookeeper_business]
corpint8    zookeeper_id=7
corpint9    zookeeper_id=5
corpint11   zookeeper_id=6

7. По очереди пересоздать zk ноды с новым zoo.cfg
ansible-playbook -i ./integration/inv-kafka-business kafka-business.yml -t zookeeper --limit 'corpint8'
ansible-playbook -i ./integration/inv-kafka-business kafka-business.yml -t zookeeper --limit 'corpint9'
ansible-playbook -i ./integration/inv-kafka-business kafka-business.yml -t zookeeper --limit 'corpint11'

--- cat /etc/zookeeper/zoo.cfg
# Server list for clusters of 3 or more hosts
server.5=corpint9:2888:3888
server.6=corpint11:2888:3888
server.7=corpint8:2888:3888

8. По очереди пересоздать кафка брокеров, чтобы брокеры использовали новый конфиг zookeeper.connect

ansible-playbook -i ./integration/inv-kafka-business kafka-business.yml -t kafka --limit 'corpint4'
ansible-playbook -i ./integration/inv-kafka-business kafka-business.yml -t kafka --limit 'corpint5'
ansible-playbook -i ./integration/inv-kafka-business kafka-business.yml -t kafka --limit 'corpint7'

```
