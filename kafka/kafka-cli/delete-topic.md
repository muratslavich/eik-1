# Delete topic



```
docker exec -it kafka bash
unset JXM_PORT

kafka-exec ./usr/bin/kafka-topics --zookeeper corpint4:2181 --list

kafka-exec ./usr/bin/kafka-topics --zookeeper corpint4:2181 --delete --topic check-EscrowOrderCheck-v1-operations-changelog
check-EscrowOrderCheck-v1
check-EscrowOrderCheck-v1-operations-changelog
check-EscrowOrderCheck-v1_deadletter
check-EscrowOrderCheck-v1_out
check-EscrowOrderCheck-v1_reprocessing
check-EscrowOrderCheck-v1_reprocessing-operations-changelog

// zkCli.sh
ls /admin/delete_topics
```
