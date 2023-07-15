# Мониторинг микросервисов

*   #### Ребуты сервисов не мониторятся, или им не придается должное значение

    Проблема: как минимум, сам ребут — это пользователь, который не получил результат, как максимум — одна и та же функция может систематически не выполняться.
* Ошибки сервисов\
  Приложение может и не падать, но выдавать пользователю (или другому приложению через API) трейс эксепшена. Таким образом, даже если мы замониторили перезагрузки приложений — мы можем не обнаружить случаи, когда выполнение запроса завершилось некорректно.
* Endpoint c health-check-ом сервиса отсутствует или не делает осмысленной работы\
  Во-первых, сам факт endpoint-а с healthcheck-ом должен стать стандартом для любого вашего сервиса, если еще не стал. Во-вторых, такой healthcheck должен осмотреть здоровье/доступность критичных для функционирования сервиса систем (доступы до очередей, БД, доступность сервисов И так далее).
*   #### Время ответа API и взаимодействия с другими сервисами не мониторится

    Используйте трейсинг. Jaeger стал практически стандартом.

    В условиях когда большинство частей приложения стало взаимодействующими между собой клиентами и серверами, API критически важно понимать, за какое время конкретный сервис выдает ответ. Если по какой-то причине оно увеличилось, суммарное время ответа приложения может превратиться в водопад задержек.
* #### Сервис — это по-прежнему приложение, а приложение — это по-прежнему память и процессор (и иногда диск)
* Появление новых сервисов следует мониторить
*   #### APM и профайлинг приложения


*   #### Бонус-трек: SRE, время ответа приложения — больше не время ответа сервера!

    Мы привыкли к тому, что замеряем производительность системы по времени ответа сервера, однако современное веб-приложение на 80% своей логикой перенесено в фронтенд. Если вы еще не замеряете время ответа приложения как время выдачи страницы с фронтенд-метриками загрузки страницы — начните прямо сейчас. Пользователю уже совершенно неважно, за 200 или 400 миллисекунд отдал сервер ответ, если фронт на Angular-е или React-е дальше отрисовывает это в течении 10 секунд.

\\





### What all metrics Spring boot 2 auto-configure? <a href="#9a5c" id="9a5c"></a>

1. JVM, report utilization:
2. CPU usage
3. Spring MVC and WebFlux request latencies
4. RestTemplate latencies
5. Cache utilization
6. Datasource utilization, including HikariCP pool metrics
7. RabbitMQ connection factories
8. File descriptor usage
9. Logback: record the number of events logged to Logback at each level
10. Uptime: report a gauge for uptime and a fixed gauge representing the application’s absolute start time
11. Tomcat usage

### Things we can monitor? <a href="#49c9" id="49c9"></a>

1. Monitoring REST API calls.
2. Monitoring external API calls.
3. Method level monitoring
4. Custom monitoring



### How to monitor your application using Micrometer, Prometheus? <a href="#bbeb" id="bbeb"></a>

We need to have the following dependencies to enable /actuator endpoints within our microservices.

```
spring-boot-starter-actuator
spring-boot-starter-web
spring-boot-starter-aop
micrometer-core
micrometer-registry-prometheus
```



Post running spring boot application with dependencies things will start working out of the box and it will expose the /actuator endpoint along with system metrics.

<pre><code>/actuator
/actuator/auditevents
/actuator/beans
/actuator/caches/{cache}
/actuator/caches
/actuator/health
/actuator/health/{component}
/actuator/health/{component}/{instance}
/actuator/conditions
/actuator/configprops
/actuator/env
/actuator/env/{toMatch}
/actuator/info
/actuator/loggers/{name}
/actuator/loggers
/actuator/heapdump
/actuator/threaddump
<strong>/actuator/prometheus
</strong>/actuator/metrics/{requiredMetricName}
<strong>/actuator/metrics
</strong>/actuator/scheduledtasks
/actuator/httptrace
/actuator/mappings
</code></pre>



Additionally, you can configure the micrometer specifying which all end-points you need to enable in production, what all metrics to emit by default, generate hibernate statistics, etc by adding them to application.properties file. Following example:

```
# Enable specific endpoints info, health, prometheus management.endpoints.web.exposure.include=*
management.endpoints.enabled-by-default=false
management.endpoint.health.enabled=true
management.endpoint.info.enabled=true
management.endpoint.prometheus.enabled=true# By default enable hibernate metrics
spring.jpa.properties.hibernate.generate_statistics=true
```



Also, you can customize the MeterRegistry either to filter a few URLs or add any specific tags by default or add default distribution statistics. You can also enable basic auth on the actuator end-points and If method-level monitoring is required then you have to configure TimedAspect which uses the pointcut expression. Following example:

```

import org.springframework.boot.actuate.autoconfigure.metrics.MeterRegistryCustomizer;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import io.micrometer.core.instrument.MeterRegistry;
import io.micrometer.core.instrument.config.MeterFilter;
import io.micrometer.core.aop.TimedAspect;
import io.micrometer.core.instrument.Meter;
import io.micrometer.core.instrument.distribution.DistributionStatisticConfig;

@Configuration
public class MonitoringConfiguration {
	/**
	 * Enable metrics registry customizer 
	 * This is to enable any customer metrics we would like to push like pushing common tags
	 * registry.config().commonTags("region", "<region name>");
	 * Also we are filtering out the URI which we don't like to index
	 * 
	 * @return MeterRegistryCustomizer<MeterRegistry> metrics registry
	 */
	@Bean
    MeterRegistryCustomizer<MeterRegistry> meterRegistryCustomizer() {
        return registry -> registry.config()
                .meterFilter(MeterFilter.deny(id -> {
                    String uri = id.getTag("uri");
                    return uri != null && uri.startsWith("/actuator");
                }))
                .meterFilter(MeterFilter.deny(id -> {
                    String uri = id.getTag("uri");
                    return uri != null && uri.contains("favicon");
                }))
                .meterFilter(new MeterFilter() {
                    @Override
                    public DistributionStatisticConfig configure(Meter.Id id, DistributionStatisticConfig config) {
                        return config.merge(DistributionStatisticConfig.builder()
                        		.percentiles(0.1, 0.25, 0.5, 0.75, 0.9, 0.95, 0.99, 0.995, 0.999)
                                .percentilesHistogram(true)
                                .build());
                    }
                });
    }
	
	/**
     * AspectJ aspect for intercepting types or method annotated with @Timed.
     * By default @Timed annotation does not work with method execution 
     * so we have to enable TimedAspect with uses AspectJ for intercepting method annotation
     * 
     * @param registry default meter registry
     * 
     * @return TimedAspect timed aspect
     */
    @Bean
    public TimedAspect timedAspect(MeterRegistry registry) {
       return new TimedAspect(registry);
    }
}
view rawMeterRegistryCustomizer.java hosted with ❤ by GitHub
```

