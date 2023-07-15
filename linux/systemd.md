# systemd

The basic component of the Linux system. It runs with PID 1, provides a system and service manager. Starts rest of the system.

* aggressive parallelization.
* uses socket and D-bus activation for starting services
* offers on-demand starting of daemons
* keeps track of processes
* maintains mount and automount points
* logging daemon
* utilities to control basic system configuration - hostname, date, locale
* maintain a list of logged users
* running containers and virtual machines
* system accounts, runtime directories and settings, and daemons to manage simple network configuration, network time synchronization, log forwarding, and name resolution.

systemd starts services described in directories:

* **`/usr/lib/systemd/system/`**
* **`/run/systemd/system/`**
* **`/etc/systemd/system/`**

Unit it's a file with simple syntax:

```
[Section_name]
key=value
```

For the simplest untit we have to describe 3 sections: \[Unit], \[Service], \[Install]:

```
[Unit]                   # unit description
Description=MyUnit
After=syslog.target      # start unit after service or group
After=network.target
After=nginx.service
After=mysql.service
Requires=mysql.service   # required run mysql service
Wants=redis.service      # desirable redis service to run

[Service]                # describe commands to start service
Type=forking
PIDFile=/work/www/myunit/shared/tmp/pids/service.pid
WorkingDirectory=/work/www/myunit/current

User=myunit              # user and group who start service
Group=myunit

Environment=RACK_ENV=production       # envs

OOMScoreAdjust=-1000                  # prevent killing service after OOM

# commands for start/stop/reload
ExecStart=/usr/local/bin/bundle exec service -C /work/www/myunit/shared/config/service.rb --daemon
ExecStop=/usr/local/bin/bundle exec service -S /work/www/myunit/shared/tmp/pids/service.state stop
ExecReload=/usr/local/bin/bundle exec service -S /work/www/myunit/shared/tmp/pids/service.state restart
TimeoutSec=300                       # How much to wait to execute
Restart=always

# multi-user.target или runlevel3.target соответствует нашему привычному runlevel=3 
# «Многопользовательский режим без графики. Пользователи, как правило, входят в систему при помощи множества консолей или через сеть»
[Install]
WantedBy=multi-user.target
```

```
$ systemctl enable myunit
$ systemctl -l status myunit
$ systemctl start myunit
$ systemctl daemon-reload
$ journalctl -u myunit -e -f
```

etc/systemd/system/kafka.service

```
[Unit]
Description=Kafka container
Requires=docker.service
After=docker.service

[Service]
Restart=always
SyslogIdentifier=kafka
ExecStartPre=-/usr/bin/docker rm kafka
ExecStart=/usr/bin/docker run --rm \
  -p 9092:9092 \
  -p 9095:9095 \
  -p 9997:9997 \
  -h corpint9.moscow.alfaintra.net \
  -v /var/log/kafka:/var/log/kafka \
  -v /data/kafka/config/log4j.properties:/etc/confluent/docker/log4j.properties.template \
  -v /data/kafka/kafka-logs-corpint9.moscow.alfaintra.net:/kafka/kafka-logs-corpint9.moscow.alfaintra.net \
  -v /data/kafka/secrets:/etc/kafka/secrets \
  -v /data/kafka/kafka_jaas.conf:/kafka/kafka_jaas.conf \
  -e KAFKA_LISTENERS=CLIENT_SSL://0.0.0.0:9095,REPLICATION://0.0.0.0:9092 \
  -e KAFKA_ADVERTISED_LISTENERS=CLIENT_SSL://corpint9.moscow.alfaintra.net:9095,REPLICATION://corpint9.moscow.alfaintra.net:9092 \
  -e JMX_PORT=9997 \
  -e KAFKA_ZOOKEEPER_CONNECT=corpint11:2181,corpint8:2181,corpint9:2181 \
  -e KAFKA_ZOOKEEPER_CONNECTION_TIMEOUT_MS=6000 \
  -e KAFKA_BROKER_ID=1005 \
  -e KAFKA_LISTENER_SECURITY_PROTOCOL_MAP=PLAINTEXT:PLAINTEXT,SASL_PLAINTEXT:SASL_PLAINTEXT,REPLICATION:PLAINTEXT,CLIENT:SASL_PLAINTEXT,CLIENT_SSL:SASL_SSL \
  -e KAFKA_INTER_BROKER_LISTENER_NAME=REPLICATION \
  -e KAFKA_SASL_ENABLED_MECHANISMS=PLAIN,SCRAM-SHA-256 \
  -e KAFKA_SASL_MECHANISM_INTER_BROKER_PROTOCOL=SCRAM-SHA-256 \
  -e KAFKA_SUPER_USERS=User:kafka;User:ANONYMOUS  \
  -e "KAFKA_OPTS=-Djava.security.auth.login.config=/kafka/kafka_jaas.conf" \
  -e KAFKA_ZOOKEEPER_SET_ACL=true \
  -e KAFKA_AUTHORIZER_CLASS_NAME=kafka.security.auth.SimpleAclAuthorizer \
  -e CONFLUENT_SUPPORT_METRICS_ENABLE=false \
  -e KAFKA_SSL_TRUSTSTORE_FILENAME=server.truststore.jks \
  -e KAFKA_SSL_KEY_CREDENTIALS=key_pwd \
  -e KAFKA_SSL_KEYSTORE_CREDENTIALS=keystore_pwd \
  -e KAFKA_SSL_TRUSTSTORE_CREDENTIALS=truststore_pwd \
  -e KAFKA_SSL_KEYSTORE_FILENAME=server.keystore.jks \
  -e "KAFKA_HEAP_OPTS=-Xmx2g -Xms2g" \
  -e KAFKA_DELETE_TOPIC_ENABLE=True \
  -e KAFKA_BROKER_ID_GENERATION_ENABLE=False \
  -e KAFKA_AUTO_CREATE_TOPICS_ENABLE=True \
  -e KAFKA_DEFAULT_REPLICATION_FACTOR=4 \
  -e KAFKA_MIN_INSYNC_REPLICAS=3 \
  -e KAFKA_INTER_BROKER_PROTOCOL_VERSION=2.1 \
  -e KAFKA_VERSION=2.1.1 \
  -e KAFKA_RESERVED_BROKER_MAX_ID=2000 \
  -e KAFKA_NUM_NETWORK_THREADS=3 \
  -e KAFKA_NUM_IO_THREADS=8 \
  -e KAFKA_NUM_PARTITIONS=4 \
  -e KAFKA_NUM_RECOVERY_THREADS_PER_DATA_DIR=1 \
  -e KAFKA_OFFSETS_RETENTION_MINUTES=43200 \
  -e KAFKA_CLEANUP_POLICY=delete \
  -e KAFKA_DELETE_RETENTION_MS=10000 \
  -e KAFKA_RETENTION_MS=1048576 \
  -e KAFKA_RETENTION_BYTES=1048576 \
  -e KAFKA_RETENTION_HOURS=168 \
  -e KAFKA_SEGMENT_MS=3600000 \
  -e KAFKA_SEGMENT_BYTES=1048576 \
  -e KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR=3 \
  -e KAFKA_LOG_MESSAGE_FORMAT_VERSION=2.1 \
  -e KAFKA_LOG_DIRS=/kafka/kafka-logs-corpint9.moscow.alfaintra.net \
  -e KAFKA_LOG_CLEANER_ENABLE=True \
  -e KAFKA_LOG_CLEANER_POLICY=delete \
  -e KAFKA_LOG_RETENTION_HOURS=168 \
  -e KAFKA_LOG_RETENTION_CHECK_INTERVAL_MS=300000 \
  -e KAFKA_LOG_SEGMENT_BYTES=1073741824 \
  -e KAFKA_SOCKET_SEND_BUFFER_BYTES=102400 \
  -e KAFKA_SOCKET_RECEIVE_BUFFER_BYTES=102400 \
  -e KAFKA_SOCKET_REQUEST_MAX_BYTES=104857600 \
  -e KAFKA_MESSAGE_MAX_BYTES=1000012 \
  -e KAFKA_NUM_REPLICA_FETCHERS=4 \
  -e "KAFKA_JMX_OPTS=-Dcom.sun.management.jmxremote -Dcom.sun.management.jmxremote.authenticate=False -Dcom.sun.management.jmxremote.ssl=False -Djava.rmi.server.hostname=corpint9 -Dcom.sun.management.jmxremote.rmi.port=9997" \
  --net=bridge \
  --name kafka infra.binary.alfabank.ru/cp-kafka:5.3.1

ExecStop=/usr/bin/docker stop -t 10 kafka

[Install]
WantedBy=multi-user.target
```
