# CAP теорема

**Теорема CAP** (известная также как **теорема Брюера**) — эвристическое утверждение о том, что в любой реализации распределённых вычислений возможно обеспечить не более двух из трёх следующих свойств:

* _согласованность данных_ (англ. consistency) — во всех вычислительных узлах в один момент времени данные не противоречат друг другу;
* _доступность_ (англ. availability) — любой запрос к распределённой системе завершается корректным откликом, однако без гарантии, что ответы всех узлов системы совпадают;
* _устойчивость к разделению_ (англ. fault tolerance) — расщепление распределённой системы на несколько изолированных секций не приводит к некорректности отклика от каждой из секций.

<figure><img src=".gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

#### Преимущества распределенных систем

* Horizontal scaling (добавление инстансов или серверов)
* Отказоустойчивость -  fault tolerance 2n+1
* Большие нагрузки - параллелизм
* Уменьшение latency размещая узлы ближе к пользователю

#### Недостатки распределенных систем

* обработка сбоев может быть затруднительной
* необходимость поддержания ACID транзакций
* поддержка безопасности данных - доступ к реплицированным данным
* инфраструктурные затраты