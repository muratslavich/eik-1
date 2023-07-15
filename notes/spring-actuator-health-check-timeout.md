# Spring Actuator health check timeout

Выставив настройку актуатора

```
management.server.port=8081
```

Приложение должно создать отдельный tomcat connector \`org.apache.catalina.connector.Connector\` с внутренним тред пулом, и не зависеть от загруженности основного приложения



По сути это отдельный контейнер сервлетов, он будет создан на каждый коннектор в `TomcatEmbeddedServletContainerFactory`



{% embed url="https://tech.asimio.net/2016/12/15/Configuring-Tomcat-to-Listen-on-Multiple-ports-using-Spring-Boot.html" %}
несколько портов
{% endembed %}

Ограничение по тредам должно быть отдельным для каждого контейнера (по умолчанию 200)

```
server.tomcat.threads.max=10
```

Т.е. 10 тредов для 8080 и 10 для 8081



Другие [spring tomcat properties](https://docs.spring.io/spring-boot/docs/2.0.9.RELEASE/reference/htmlsingle/#common-application-properties)

```properties
server.tomcat.max-connections=10000 # Maximum number of connections that the server accepts and processes at any given time.
server.tomcat.max-threads=200 # Maximum amount of worker threads.
server.tomcat.min-spare-threads=10 # Minimum amount of worker threads.
server.tomcat.accept-count=100 # Maximum queue length for incoming connection requests when all possible request processing threads are in use.
```



Tomcat has a default thread pool size of 200 **per** Connector



Besides the threads in the **connector thread** pools **Tomcat may create other threads**, and of course the **JVM itself has own threads** e.g. for the garbage collector.



@v3ga I guess Tomcat puts connector threads into a ThreadGroup. You could use [stackoverflow.com/a/1323480/3215527](http://stackoverflow.com/a/1323480/3215527) to examine the thread groups and based on that answer your questions – [wero](https://stackoverflow.com/users/3215527/wero) [Apr 9, 2016 at 15:26](https://stackoverflow.com/questions/36518444/number-of-threads-run-by-the-process-is-more-than-tomcat-maxthreads#comment60642943\_36518653)



however it might not solve your root cause of all worker threads being occupied when **there is a burst of traffic**. Perhaps you need to look into something like rate limiting to deal with such scenario, so your service is not getting more traffic than it can handle.



In the end, we attacked the root of the problem, which was resource starvation within the pod. We found that the apps experiencing the health check timeouts had lots of extra threads for various 3rd party thread pools. In some cases we had apps with close to 500 threads, so even under what we considered moderate load, the tomcat pools would get starved and couldn't handle new requests.

FWIW, the biggest culprit we found was the effect of [CPU request](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/) on a pod and the JDK. When we _didn't_ set any request, the JDK would see every CPU on the _node_ when it queried for numbers of processors. We found there are lots of places in the Java ecosystem where number of processors is used to initialize different thread pools.



Может  GC отъедает треды?

* \-XX:ParallelGCThreads: Sets the number of threads used during parallel phases of the garbage collectors. The default value varies with the platform on which the JVM is running.
* \-XX:ConcGCThreads: Number of threads concurrent garbage collectors will use. The default value varies with the platform on which the JVM is running.

