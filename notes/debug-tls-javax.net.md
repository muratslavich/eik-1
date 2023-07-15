# Debug tls javax.net

Для дебага джава приложения





{% embed url="https://stackoverflow.com/questions/23659564/limiting-java-ssl-debug-logging" %}

### Resolution

* You can use the following as a java argument when starting a standalone Java client.

[Raw](https://access.redhat.com/solutions/973783)

```
 -Djavax.net.debug=ssl,handshake
```

* To get more filtered logging you can use:

[Raw](https://access.redhat.com/solutions/973783)

```
-Djavax.net.debug=ssl:handshake:verbose:keymanager:trustmanager -Djava.security.debug=access:stack
```

* To test the same with an uploaded [pure java example client](https://access.redhat.com/site/node/973783/40/0), you will need to run it using the following command:

[Raw](https://access.redhat.com/solutions/973783)

```
java -Djavax.net.debug=ssl:handshake:verbose:keymanager:trustmanager -Djava.security.debug=access:stack  JavaHttpsClient https://example.com:port 1
```

* To get decrypted HTTP requests/responses:

[Raw](https://access.redhat.com/solutions/973783)

```
java -Djavax.net.debug=ssl:record:plaintext  JavaHttpsClient https://example.com:port 1
```

**NOTE :**`https://example.com:port` is the server being invoked host and `HTTPS` port. This can also be anything like `https://www.redhat.com`. Also, "1" means the number of calls. In the above example it is only a single call.

* To show available options which can be set to `javax.net.debug`:

[Raw](https://access.redhat.com/solutions/973783)

```
java -Djavax.net.debug=help JavaHttpsClient https://redhat.com/ 1
```
