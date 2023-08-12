# Зоопарк клиентов

Нативные от elasticsearch

* [Java High Level REST Client](https://www.elastic.co/guide/en/elasticsearch/client/java-rest/current/java-rest-high.html#java-rest-high) deprecated since 7.15
* [Java Transport Client](https://www.elastic.co/guide/en/elasticsearch/client/java-api/7.17/index.html) deprecated
* [Elasticsearch Java API Client](https://www.elastic.co/guide/en/elasticsearch/client/java-api-client/current/getting-started-java.html)  RestClient



Spring Data [https://docs.spring.io/spring-data/elasticsearch/docs/current/reference/html/](https://docs.spring.io/spring-data/elasticsearch/docs/current/reference/html/)

мы используем версию [https://docs.spring.io/spring-data/elasticsearch/docs/4.4.13/reference/html/#elasticsearch.clients](https://docs.spring.io/spring-data/elasticsearch/docs/4.4.13/reference/html/#elasticsearch.clients)

* Imperative Rest Client
* Reactive Rest Client
* High Level REST Client (deprecated)
* Reactive Client (deprecated).  -  наш



```kotlin
@Configuration
class ElasticsearchConfiguration(
    private val properties: ElasticsearchConnectionProperties,
) : AbstractReactiveElasticsearchConfiguration() {
    @Bean
    fun analyzer() = SimpleRussianAnalyzer()

    override fun reactiveElasticsearchClient(): ReactiveElasticsearchClient = ClientConfiguration.builder()
        .connectedTo(*properties.nodeList.toTypedArray())
        .apply {
            when {
                properties.sslEnabled -> applySslContext()
            }
        }
        .withBasicAuth(properties.user, properties.password)
        .build()
        .let { ReactiveRestClients.create(it) }

    private fun ClientConfiguration.MaybeSecureClientConfigurationBuilder.applySslContext() {
        val sslContext = SSLContextBuilder
            .create()
            .loadTrustMaterial(
                ResourceUtils.getFile(System.getenv("TRUSTSTORE_LOCATION")),
                System.getenv("TRUSTSTORE_PASSWORD").toCharArray()
            )
            .build()
        this.usingSsl(sslContext)
    }
}
```
