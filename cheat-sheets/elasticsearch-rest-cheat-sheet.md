# Elasticsearch rest cheat sheet

### Elasticsearch Indices

#### List All Indices:

```
curl -X GET ‘http://localhost:9200/_cat/indices?v
```

or

```
curl -v "localhost:9200/_cat/indices"
```

\#You can also do this in Kibana Console by querying `GET _cat/indices`.

#### Delete an Elasticsearch Index

```
curl -X DELETE 'http://localhost:9200/examples'
```

#### Back up an Elasticsearch Index

```
curl -XPOST --header 'Content-Type: application/json' http://localhost:9200/_reindex -d '{
  "source": {
    "index1": "someexamples"
  },
  "dest": {
    "index2": "someexamples_copy"
  }
}'
```

#### List All Docs in an Index:

```
curl -X GET 'http://localhost:9200/elasticsearch_query_examples/_search'
```

#### List All Data

```
curl -XPUT --header 'Content-Type: application/json' http://localhost:9200/elasticsearch_query_examples/_doc/1 -d '{
   "value1" : "value2"
}'
```

### Elasticsearch Queries

There are a few different basic commands for querying Elasticsearch. For more info on this, check out our rundown of [Elasticsearch queries](https://logz.io/blog/elasticsearch-queries/) and the Elasticsearch query API.

#### Query with URL Parameters

```
curl -X GET http://localhost:9200/elasticsearch_query_examples/_search?q=hypocrite_senator
```

### Query with Elasticsearch Query DSL

```
curl -XGET --header 'Content-Type: application/json' http://localhost:9200/examples/_search -d '{
      "query" : {
        "match" : { "hypocrite_senator": "graham-lindsey" }
    }
}'
```

#### By Date

```
curl GET filebeat-7.10.0-2020.11.03-000001/_search
 {
    "query": {
        "range" : {
            "timestamp": {
              "event.created": {
                  "time_zone": "+02:00"
                  "gte" : "now-15d/d"
                  "lt" : “now” 
              }
            }
        }
}
}
```

### Elasticsearch Clusters

#### Move shards from one node to another

**When to do it:** When too many hot shards reside in one data node and you want to spread them out manually. Elasticsearch does not take these things into consideration when placing shards across the cluster, so sometimes it is necessary to move them manually.\
**The cURL command:**\


```
curl -XPOST 'http://localhost:9200/_cluster/reroute' -d '{
"commands" : [
{
"move" :
{
"index" : "indexname", "shard" : 1,
"from_node" : "nodename", "to_node" : "nodename"
}
}
]
}';echo
```



#### Force the allocation of an unassigned shard with a reason

**When to do it:** Sometimes you have unassigned shards in a cluster, but you just cannot figure out why. It can have plenty of causes such as lack of space or shard awareness. This will output a lot of verbose data. If you look at the end of the output, you will see the reason for the non-allocation. (Note: The “?explain” part can be also applied to the previous curl to get a reason if you cannot move a shard around.)\
**The cURL command:**\


```
curl -XPOST 'http://localhost:9200/_cluster/reroute?explain' -d '{
"commands" : [ {
"allocate" : {
"index" : "indexname", "shard" : 0, "node" : "nodename"
}
} ]
}';echo
```

#### Remove nodes from clusters gracefully

**When to do it:** When you want to decommission a node or perform any type of maintenance without the cluster turning yellow or red (depending on your replicas settings). Note: If you drain a node and want to return it to the cluster afterward, you need to call that endpoint again with the IP field blank.\
**The cURL command:**\


```
curl -XPUT localhost:9200/_cluster/settings -d '{
"transient" :{
"cluster.routing.allocation.exclude._ip" : "1.2.3.4"
}
}';echo
```

#### Force a synced flush

**When to do it:** Before you restart a node that you are not gracefully removing from the cluster. This will place a sync ID on all indices, and as long as you are not writing to them, the recovery time of those shards will be significantly faster.\
**The cURL command:**

```
curl -XPOST 'localhost:9200/_flush/synced'
```

#### Change the number of moving shards to balance the cluster

**When to do it:** Setting the cURL command (see below) to 0 will be useful if you have a planned maintenance and do not want the cluster to start to move shards under your feet. Setting a higher value will help to rebalance the cluster when a new node joins it.\
**The cURL command:**

```
curl -XPUT localhost:9200/_cluster/settings -d '{
"transient" :{
"cluster.routing.allocation.cluster_concurrent_rebalance" : 2
}
}';echo
```

#### Change the number of shards being recovered simultaneously per node

**When to do it:** If a node has been disconnected from the cluster, all of its shards will be unassigned. After a certain delay, the shards will be allocated somewhere else. The number of concurrent shards per node that will be recovered is determined by that setting.\
**The cURL command:**

```
curl -XPUT localhost:9200/_cluster/settings -d '{
"transient" :{
"cluster.routing.allocation.node_concurrent_recoveries" : 6
}
}';echo
```

#### Change the recovery speed

**When to do it:** To avoid overloading the cluster, Elasticsearch limits the speed that is allocated to recovery. You can carefully change that setting to make it recover more quickly.\
**The cURL command:**\


```
curl -XPUT localhost:9200/_cluster/settings -d '{
"transient" :{
"indices.recovery.max_bytes_per_sec" : "80mb"
}
}';echo
```



#### Change the number of concurrent streams for a recovery on a single node

**When to do it:** If a node has failed and you want to speed up recovery, you can increase this setting. Make sure to monitor the cluster so that you will not end uploading it too much.\
**The cURL command:**

```
curl -XPUT localhost:9200/_cluster/settings -d '{
"transient" :{
"indices.recovery.concurrent_streams" : 6
}
}';echo
```



#### Change the size of the search queue

**When to do it:** If your cluster is loaded and takes too much time to answer search queries, you can carefully increase that setting so that you will not drop searches. (If you see an increase in the “rejected” metric for any queue, this recommendation is applicable.)\
**The cURL command:**\


```
curl -XPUT localhost:9200/_cluster/settings -d '{
"transient" :{
"threadpool.search.queue_size" : 2000
}
}';echo
```



#### Clear the cache on a node

**When to do it:** If a node reaches a high JVM value, you can call that API as an immediate action on a node level to make Elasticsearch drop caches. It will hurt performance, but it can save you from OOM (Out Of Memory).\
**The cURL command:**\


```
curl -XPOST 'http://localhost:9200/_cache/clear'
```



#### Adjust the circuit breakers

**When to do it:** To avoid not getting to OOM in Elasticsearch, you can tweak the settings on the circuit breakers. This will limit the search memory and drop all searches that are estimated to consume more memory than that desired level. You can read more about that here. Note: This is a really delicate setting that you need to calibrate carefully.\
**The cURL command:**\


```
curl -XPUT localhost:9200/_cluster/settings -d '{
"persistent" : {
"indices.breaker.total.limit" : "40%"
}
}'; echo
```



#### Showing Cluster Health

```
curl --user $pwd  -H 'Content-Type: application/json' -XGET https://1234567876543219876567890.eu-central-1.aws.cloud.es.io:1234/_cluster/health?pretty
```



You can get a peek at your Elasticsearch cluster’s health by calling the Health API. Get back the status color for shard levels and index levels (green, all allocated; yellow, primary allocated by not replicas; red, specific shard is unallocated). Index level is evaluated by the worst shard; cluster status is then evaluated by worst index.
