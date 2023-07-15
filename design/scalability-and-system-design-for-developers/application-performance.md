# Application performance

* Profiling - is the dynamic analysis of our code. It helps us measure the space and the time complexity of our code and enables us to figure out issues like concurrency errors, memory errors and robustness.
* Monitoring - Cadvisor, Prometheus and Grafana.
* Caching - _Cache_ wisely, and cache everywhere. Cache all the static content. Hit the database only when it is really required. Try to serve all the read requests from the cache. Use a write-through cache.
* CDN - reduces the latency of the application due to the proximity of the data from the requesting user.
* Data compression - compressed data consumes less bandwidth.
* Reduce client requests.





During the load and stress testing, different system parameters are watched  :

* CPU usage
* network bandwidth consumption
* throughput
* _number of requests processed within a stipulated time_
* latency
* memory usage
* end-user experience when the system is under heavy load etc.
