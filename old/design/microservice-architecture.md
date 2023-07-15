# Microservice architecture

\


| Advantages                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | Disadvantages                                                                                                                                                                                                                                                                                                                                                                                          |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| <ul><li>Автономные, независимые модули легче развертывать</li><li>Меньше опасность сбоев, отказоустойчивость</li><li>Семантически каждый сервис легче</li><li>Легче мастштабировать - можно масштабировать по необходимости</li><li>Легче расширять систему - разрабатывать новые сервисы</li><li>Легче разрабатывать функционал индивидуально сервиса</li><li>Легче тестировать каждый сервис - меньше зависимостей</li><li>Быстрее релизный цикл, меньший риск внести фатальное изменение</li><li>Новичкам легче</li></ul><p><br></p> | <ul><li>сложности распределенной системы (консистентность данных, распределенные транзакции)</li><li>высокая вероятность сбоев во время коммуникаций между сервисами</li><li>network latency and load balancing - сквозные запросы, балансировщики</li><li>сложнее организовать инфраструктуру (тесты, развертывания)</li><li>дороже для заказчика</li><li>Сложнее поддерживать безопасность</li></ul> |

Сложность микросервисной архитектуры

* ограниченный контекст
* динамическое масштабирование
* мониторинг
* **отказоустойчивость High Availability**
* цикличесике зависимости
* devops инфраструктура

\
DDD - domain driven design - предметно-ориентированное программирование.



\


| Monolith                                                                                                                                                                               | Microservice                                                             |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| Disadvantages                                                                                                                                                                          |                                                                          |
| Complexity and cost of a single server, computing, memory, and network.                                                                                                                | Adds complexity to the architecture.                                     |
| Impossibility to scale concrete feature of an application.                                                                                                                             | Distributed transactions.                                                |
| Run in a single process limits incoming connections and operations. However, it's possible to scale the whole application, another pricey solution.                                    | More difficult to maintain.                                              |
| Upgrades, patches, and migrations impact clients. It's difficult to maintain monolith applications in a highly available active/passive configuration.Downtime and service disruption. | <p><br></p>                                                              |
| Difficult to have separated teams.                                                                                                                                                     | <p><br></p>                                                              |
| Advantages                                                                                                                                                                             |                                                                          |
| Easy to scale, you need only a load balancer.                                                                                                                                          | Hardware not so strict and expensive.                                    |
| Easy to develop, you can start quickly instead of thinking about cross-service interactions.                                                                                           | Can be deployed separately.                                              |
| Easy to deploy.                                                                                                                                                                        | Can use different languages, suitable for the concrete issues.           |
| <p><br></p>                                                                                                                                                                            | Scalability, each microservice can be scaled individually and automated. |
| <p><br></p>                                                                                                                                                                            | No downtime and service disruption.                                      |
| <p><br></p>                                                                                                                                                                            | Fast delivering and having separate teams for features.                  |

\
\
