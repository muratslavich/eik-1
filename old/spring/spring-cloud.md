# Spring Cloud

* Netflix OSS
  * **Eurika**
  * Hystrix Stream, Hystrix Dashboard (ui for metrics)
  * **Turbine**
* Spring Cloud Sleuth - distributed tracing
  * Zipkin - ui for tracing
* Spring Cloud Security
  * OAuth2 + JWT, OpenId + SSO
* Spring Cloud Streams - messaging supports kafka/rabbit
* Spring Cloud Bus
* Spring Cloud Config - config server

**Discovery Server**

```
@EnableEurekaServer // @EnableDiscoveryServer
@SpringBootApplication
public class ServiceRegistrationAndDiscoveryServiceApplication {

	public static void main(String[] args) {
		SpringApplication.run(ServiceRegistrationAndDiscoveryServiceApplication.class, args);
	}
}

// application.yaml
server:
  port: ${PORT:8761} # Indicate the default PORT where this service will be started
eureka:
  client:
    registerWithEureka: false   #telling the server not to register himself in the service registry
    fetchRegistry: false
  server:
    waitTimeInMsWhenSyncEmpty: 0    #wait time for subsequent sync

// bootstrap.yaml
spring:
  application:
    name: eureka
  cloud:
    config:
      uri: ${CONFIG_SERVER_URL:http://localhost:8888}
```

Client

```
@EnableDiscoveryClient
@SpringBootApplication
public class ServiceRegistrationAndDiscoveryClientApplication {

	public static void main(String[] args) {
		SpringApplication.run(ServiceRegistrationAndDiscoveryClientApplication.class, args);
	}
}

// application.yaml
server:
  port: 9098    #port number
eureka:
  instance:
    leaseRenewalIntervalInSeconds: 1
    leaseExpirationDurationInSeconds: 2
  client:
    serviceUrl:
      defaultZone: http://127.0.0.1:8761/eureka/
    healthcheck:
      enabled: true
    lease:
      duration: 5
spring:
  application:
    name: school-service    #service name
logging:
  level:
    com.example.howtodoinjava: DEBUG
```

**Ribbon** - client side load balancer - wrapper  on RestTemplate

```
@Bean
@LoadBalanced
public RestTemplate restTempl() {
    return new RestTemplate();
}
```

**Hystrix** - latency tolerance, fault tolerance

wrap all requests and control them for fast falling ![Application Resiliency Using Netflix Hystrix](https://tech.ebayinc.com/assets/Uploads/Blog/2015/08/circuit\_breaker\_state\_diagram.gif)[558 × 301](https://www.google.com/url?sa=i\&url=https%3A%2F%2Ftech.ebayinc.com%2Fengineering%2Fapplication-resiliency-using-netflix-hystrix%2F\&psig=AOvVaw3yV133wbxMv5YQ8L7ALCuU\&ust=1587753470180000\&source=images\&cd=vfe\&ved=0CAIQjRxqFwoTCKCV0N-Y\_-gCFQAAAAAdAAAAABAJ)

```
@EnableHystrix
@RestController
public class SomeController {
    
    @HystrixCommand(fallbackMethod = "fallback")
    @RequestMapping
    public String hello () {}

    private String fallback () {return "Sorry"}
}
```

Hystrix Dashboard

```
@SpringBootApplication
@EnableHystrixDashboard
... main
```

**Feign**

* Integrated with Eurika, Ribbon, Hystrix
* @EnableFeignClient

```
@EnableFeignClient(name = "HelloService") // name of needed microservice
public interface HelloClient {
    @RequestMapping("/hello")
    String sayHello(@RequestParam String name);
}
```

**Zuul**

* integrated with eurika, ribbon, Hystrix

```
@SpringBootApplication
@EnableZuulProxy
public class ApiProxy {
    ... main method
}

// application.yaml
zuul:
    ignoredServices: '*'
    routes:
        hello: 
            path: /hello/**
            serviceId: HelloService
            stripPrefix: true
        hello-world:
            path: /world/**
            serviceId: HelloWorldService
            stripPrefix: true
```
