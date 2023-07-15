# Zookeeper docker container



```
ExecStart=/usr/bin/docker run --rm \
  --memory=1024M \
  --memory-swap=1024M \
  -v /data/u_m15rt/log/zookeeper:/var/log/zookeeper \
  -v /data/u_m15rt/data/zookeeper:/var/lib/zookeeper \
  -v /data/u_m15rt/etc/zookeeper:/etc/zookeeper \
  -v /etc/hosts:/etc/hosts \
  -v /data/u_m15rt/etc/zookeeper/zoo.cfg:/etc/confluent/docker/zookeeper.properties.template \
  -v /data/u_m15rt/etc/zookeeper/log4j.properties:/etc/confluent/docker/log4j.properties.template \
  -e "KAFKA_OPTS=-Djava.security.auth.login.config=/etc/zookeeper/zookeeper_jaas.conf -Dzookeeper.DigestAuthenticationProvider.superDigest=/etc/zookeeper/secrets -Dquorum.auth.enableSasl=true -Dquorum.auth.learnerRequireSasl=true -Dquorum.auth.serverRequireSasl=true -Dquorum.cnxn.threads.size=20 -DjaasLoginRenew=3600000 -DrequireClientAuthScheme=sasl -Dquorum.auth.learner.loginContext=QuorumLearner -Dquorum.auth.server.loginContext=QuorumServer" \
  -e ZOOKEEPER_CLIENT_PORT=5151 \
  --name zookeeper_um \
  --net host \
  infra.binary.alfabank.ru/cp-zookeeper:5.3.1
```
