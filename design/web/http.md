# HTTP

Http is a protocol for transferring resources such as html or json.

{% embed url="https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview" %}

Requests are initiated by the recipient.

Application layer protocol that sents over TCP or TLS/TCP.

#### HTTP is extensible

HTTP [headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers) make it by agreement between a client and a server about headers semantic.

#### HTTP is stateless, but not sessionless

No state between two requests. To interact between two requests coherently exists HTTP cookies. Cookies allow share context.

#### HTTP connection

Connection is controlled on the transport layer by TCP, therefore not managed by HTTP.&#x20;

* HTTP/1.0 - open separate TCP connection for each request
* HTTP/1.1 - pipelining, TCP connection partially controlled by Connection header
* HTTP/2 - multiplexing messages, keep connection warm

```
GET / HTTP/1.1
Host: developer.mozilla.org
Accept-Language: fr
```

```
HTTP/1.1 200 OK
Date: Sat, 09 Oct 2010 14:28:02 GMT
Server: Apache
Last-Modified: Tue, 01 Dec 2009 20:18:22 GMT
ETag: "51142bc1-7449-479b075b2891b"
Accept-Ranges: bytes
Content-Length: 29769
Content-Type: text/html

<!DOCTYPE html... (here come the 29769 bytes of the requested web page)
```

### Methods

[`GET`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/GET) The `GET` method requests a representation of the specified resource. Requests using `GET` should only retrieve data.

[`HEAD`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/HEAD) The `HEAD` method asks for a response identical to a `GET` request, but without the response body.

[`POST`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST) The `POST` method submits an entity to the specified resource, often causing a change in state or side effects on the server.

[`PUT`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/PUT) The `PUT` method replaces all current representations of the target resource with the request payload.

[`DELETE`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/DELETE) The `DELETE` method deletes the specified resource.

[`CONNECT`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/CONNECT) The `CONNECT` method establishes a tunnel to the server identified by the target resource.

[`OPTIONS`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/OPTIONS) The `OPTIONS` method describes the communication options for the target resource.

[`TRACE`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/TRACE) The `TRACE` method performs a message loop-back test along the path to the target resource.

[`PATCH`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/PATCH) The `PATCH` method applies partial modifications to a resource.

### Responses

1. [Informational responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#information\_responses) (`100`–`199`)
2. [Successful responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#successful\_responses) (`200`–`299`)
3. [Redirection messages](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#redirection\_messages) (`300`–`399`)
4. [Client error responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#client\_error\_responses) (`400`–`499`)
5. [Server error responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#server\_error\_responses) (`500`–`599`)



**HTTP server pull** - GET

If there are no updates on the server, and the client doesn't know that, the client would keep sending requests to the server over and over.

Excessive pulls load the server and potentially can bring it down.&#x20;

* GET
* Ajax

<details>

<summary>AJAX - Asynchronous JavaScript and XML</summary>

<img src="./../.gitbook/assets/image (24).png" alt="" data-size="original">

AJAX uses an _XMLHttpRequest_ object for sending the requests to the server which is built-in the browser and uses JavaScript to update the _HTML DOM_.

</details>

Time to Live (**TTL**) requests - if the client doesn't receive a response connection will be killed by the browser. `Cache-Control: max-age`

**HTTP server push** - requests initiated by the server. After the first request, the server keeps the new updates to the client whenever they are available.

In this case, we need to **persist the connection** for further requests. The connection stays open with the help of Heartbeat Interceptors. Just a **blank request** to prevent killing connection by **the browser**.

It consumes a lot of resources, however vital in specific cases (ex: browser games).

* **W**[**ebsockets**](websocket.md) - bidirectional, over TCP, both sides should support it.
* **Long polling** - server doesn't return an empty response and hold connection. When connection breaks (because of TTL), the client has to re-establish it (browser do it automatically). The connection in long polling **stays open a bit longer** compared to polling.
* HTML5 Event Source [**Server-Sent Events**](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent\_events) - SSE server push event to the client. Connection should be established by client, the data flow is in one direction.
* Message queues
* HTML5 [HTTP streaming](https://developer.mozilla.org/en-US/docs/Web/API/Streams\_API/Concepts) - for large data over HTTP. Ex: for watching partially downloaded video.
