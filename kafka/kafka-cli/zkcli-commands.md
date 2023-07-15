# zkCLi commands

use zookeeper docker container - [https://hub.docker.com/\_/zookeeper](https://hub.docker.com/\_/zookeeper)

```
docker run --rm -it --dns-search=moscow.alfaintra.net zookeeper zkCli.sh -server "corpint4:2181,corpint5:2181,corpint7:2181"
```

connect (link) to local zk upped in compose network

```
docker run --rm -it --link kowl_kafka_zoo1_1:zookeeper --network=kowl_kafka_default zookeeper zkCli.sh -server zookeeper
```

zkCli commands

```
history

connect host:port
close

quit

config [-c] [-w] [-s]
reconfig [-s] [-v version] [[-file path] | [-members serverID=host:port1:port2;port3[,...]*]] | [-add serverId=host:port1:port2;port3[,...]]* [-remove serverId[,...]*]

ls [-s] [-w] [-R] path
stat [-w] path

get [-s] [-w] path
getAcl [-s] path

set [-s] [-v version] path data
setAcl [-s] [-v version] path acl

create [-s] [-e] [-c] [-t ttl] path [data] [acl]

delete [-v version] path
deleteall path

addauth scheme auth

listquota path
setquota -n|-b val path
delquota [-n|-b] path

printwatches on|off
removewatches path [-c|-d|-a] [-l]

redo cmdno

sync path
```

List of topics

`ls /brokers/topics`

List of brokers

`ls /brokers/ids`

Broker information

`get /brokers/ids/1001`

```
{
  "listener_security_protocol_map": {
    "CLIENT_SSL": "SASL_SSL",
    "REPLICATION": "PLAINTEXT"
  },
  "endpoints": [
    "CLIENT_SSL://corpint4.moscow.alfaintra.net:9082",
    "REPLICATION://corpint4.moscow.alfaintra.net:9083"
  ],
  "jmx_port": 9988,
  "host": "corpint4.moscow.alfaintra.net",
  "timestamp": "1628212998689",
  "port": 9083,
  "version": 4
}
```
