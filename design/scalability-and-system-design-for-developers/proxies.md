# Proxies

#### [Proxies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview#proxies) <a href="#proxies" id="proxies"></a>

Between the Web browser and the server, numerous computers and machines relay the HTTP messages. Due to the layered structure of the Web stack, most of these operate at the transport, network or physical levels, becoming transparent at the HTTP layer and potentially having a significant impact on performance. Those operating at the application layers are generally called **proxies**. These can be transparent, forwarding on the requests they receive without altering them in any way, or non-transparent, in which case they will change the request in some way before passing it along to the server. Proxies may perform numerous functions:

* caching (the cache can be public or private, like the browser cache)
* filtering (like an antivirus scan or parental controls)
* load balancing (to allow multiple servers to serve different requests)
* authentication (to control access to different resources)
* logging (allowing the storage of historical information)
