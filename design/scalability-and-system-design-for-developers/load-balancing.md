# Load balancing

* distribute heavy traffic across the servers running in the cluster based on several algorithms.
* if the node goes down LB routes the future requests to other up and running nodes in the cluster.
* single point of contact for all the client requests.
* LB can be set up to manage traffic towards any component (back app, databases, queues, and others)



### Health checks

To ensure LB route requests to up and running servers, LB regularly performs health checks.

![](<./../.gitbook/assets/image (29).png>)

* in-service machines
* out of service



### Load balancing types

* DNS Load balancing
* Hardware-based load balancing
* Software-based load balancing

### DNS load balancing

When a large-scale service such as [_amazon.com_](http://amazon.com/) runs, it needs way more than a single machine to run its services. A service as big as [_amazon.com_](http://amazon.com/) is deployed across multiple data centers in different geographical locations across the globe.

To spread the user traffic **across different clusters in different data centers**.

* Enable [The authoritative server](../../web/dns.md) to return **the list of IP addresses** of a domain to the clients.
* With every request, the authoritative server changes the order of the IP addresses in the list in a round-robin manner.
* The Client sends a request to the first IP address on the list&#x20;
* In case if the first doesn't return a response, the client can send a request to the next one.&#x20;

{% embed url="https://en.wikipedia.org/wiki/Round-robin_DNS" %}

#### Limitations

* Doesn't take into account
  * existing load on the server
  * content
  * processing time
  * out of service status
* Client can cache IP addresses



### Software load balancers

They consider many parameters such as&#x20;

* _content hosted by the servers,_&#x20;
* _cookies,_&#x20;
* _HTTP headers,_&#x20;
* _CPU and memory utilization,_&#x20;
* _load on the network_,
* healt check status,
* and so on to route traffic across the servers.

[HAProxy](https://www.haproxy.com/) is one example of a _software load balancer_ that is used widely.

They can use not only round robin, but also:

* Weighted round robin - for servers with different compute capacity.
* least connections - for a lot of long opened connections
* random
* hash source IP - when server holds client session data in memory
* hash a URL - to ensure that URL hit a certain cache that already has data on it.

