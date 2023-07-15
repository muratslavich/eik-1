# REST

Representational State Transfer - architecture, principles for building distributed services.\


* Масштабируемости взаимодействия компонентов системы (приложения)
* Общность интерфейсов
* Независимое внедрение компонентов
* Промежуточные компоненты, снижающие задержку, усиливающие безопасность

\


## Когда использовать REST?

* Когда есть ограничение пропускной способности соединения
* Если необходимо кэшировать запросы
* Если система предполагает значительное масштабирование
* В сервисах, использующих AJAX

\


## Преимущества REST:

* Отсутствие дополнительных внутренних прослоек, что означает передачу данных в том же виде, что и сами данные. Т.е. данные не оборачиваются в [XML](https://ru.wikipedia.org/wiki/XML), как это делает [SOAP](https://ru.wikipedia.org/wiki/SOAP) и [XML-RPC](https://ru.wikipedia.org/wiki/XML-RPC), не используется [AMF](https://ru.wikipedia.org/wiki/Action\_Message\_Format), как это делает Flash и т.д. Просто отдаются сами данные.
* Каждая единица информации (**ресурс**) однозначно определяется URL — это значит, что URL по сути является первичным ключом для единицы данных. Причем совершенно не имеет значения, в каком формате находятся данные по адресу — это может быть и HTML, и jpeg, и документ Microsoft Word.
* Как происходит управление информацией ресурса — это целиком и полностью основывается на протоколе передачи данных. Наиболее распространенный протокол конечно же [HTTP](https://ru.wikipedia.org/wiki/HTTP). Для HTTP действие над данными задается с помощью методов : GET (получить), PUT (добавить, заменить), POST (добавить, изменить, удалить), DELETE (удалить). Таким образом, действия [CRUD](https://ru.wikipedia.org/wiki/CRUD) (Create-Read-Update-Delete) могут выполняться как со всеми 4-мя методами, так и только с помощью GET и POST.

## Что такое RESTful

* Client-Server
* **Stateless** - server shouldn't keep any information about the client. All required information keep in request. So, every time a client interacts with the backend, the client has to send all required information, for example, authentication information.
* Cache - all request mark as cachable or not.
* Uniform interface - between services
  * _Identification of resources - URI._ Each resource in REST must have URI.
  * _Manipulation of resources through representations -_ representation of resource is current state of resource.
  * _Self-descriptive messages -_ request and response are contain all required information for handling.
  * _HATEOAS - hypermedia as the engine of application state - status of resource is passed through http body._
* Layered system - system is divided to component and each component can negotiate directly only with next level component
* Code-on-demand - code can be executed on the client side

\


## Идемпотентность

С точки зрения RESTful-сервиса, операция (или вызов сервиса) идемпотентна тогда, когда клиенты могут делать один и тот же вызов неоднократно при одном и том же результате на сервере. Другими словами, создание большого количества идентичных запросов имеет такой же эффект, как и один запрос.

POST not idempotents

### REST endpoint

* endpoints are nouns
* use plural form&#x20;
* versions in uri or in headers

• /farmers \
• /farmers/{farmer\_id} \
• /crops \
• /crops/{crop\_id}\
\- host/v2/farmers

GET /accounts/5555 HTTP/1.1 \
Accept: application/vnd.farmers.v2+json

