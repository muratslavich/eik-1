# Logging



## Logging

STDIN, STDOUT, and STDERR - standard Unix streams

By default docker logs show STDOUT and STDERR

* Logging driver (syslog) may sends logs to file.
* Non-interactive processes like db and web servers may send theirs output to log file instead of STDOUT

docker logs

docker service logs - all logs from all containers participating in service

`daemon.json`  file , which is located in `/etc/docker/`

```
{
  "log-driver": "syslog"
}
```

![](https://jaxenter.com/wp-content/uploads/2017/09/Screen-Shot-2017-09-11-at-3.08.50-PM.png)

If you do not specify a logging driver, the default is json-file

```
$ docker info --format '{{.LoggingDriver}}'
```

Example

```
$ docker logs --tail=40 -f $(docker ps | grep payments | awk '{print $1}')
```
