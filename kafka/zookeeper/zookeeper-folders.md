# Zookeeper folders

zoo.cfg

```
$ cat {{ zookeeper_config_dir }}/zoo.cfg
```

host folders

```
zookeeper_data_dir:       "/data/u_m15rt/data/zookeeper"
zookeeper_config_dir:     "/data/u_m15rt/etc/zookeeper"
zookeeper_log_dir:        "/data/u_m15rt/log/zookeeper"
```

docker volumes

```
-v {{ zookeeper_log_dir }}:/var/log/zookeeper \
-v {{ zookeeper_data_dir }}:/var/lib/zookeeper \
-v {{ zookeeper_config_dir }}:/etc/zookeeper \
-v {{ zookeeper_config_dir }}/zoo.cfg:{{ confluent_template_dir }}/zookeeper.properties.template \
-v {{ zookeeper_config_dir }}/log4j.properties:{{ confluent_template_dir }}/log4j.properties.template \
```
