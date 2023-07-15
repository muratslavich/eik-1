# alfa logging

*   Всем привет\
    Кто-нибудь знает, как подружить \``ru.alfalab.starter.logging:spring-boot-starter-alfalab-logging`\` с \``ru.alfabank.exceptional:spring-web`\`?\
    А то у меня пустые строки с логами при ошибках\
    \


    ````
    ```
    2023-07-13 17:23:34.681 ERROR [corp-sbp-b2c-api,bb2964c98d09c2e3,bb2964c98d09c2e3] 34826 --- [ctor-http-nio-4] r.a.s.l.http.reactive.LoggingFilter      : Server response

    ```
    ````
* Исхаков Фан Радисович @fiskhakov17:31alfalab:\
  \# remove when replaced with exceptional starters in dependency pack\
  errors-handling:\
  enabled: false\
  logging:\
  http:\
  ignored-uris:\
  \- "/admin.\*"\
  \- "/swagger.\*"\
  servlet:\
  enabled: true\
  extended-logging-enabled: true\
  enable-one-log-per-message: true
*   Лавриненко Дмитрий Евгеньевич @delavrinenko17:33У меня вот так выглядит конфиг\


    ````
    ```
    alfalab:
      # remove when replaced with exceptional starters in dependency pack
      errors-handling:
        enabled: false
      logging:
        ws.enabled: false
        http:
          ignoredUris:
            - "/admin/.*"
          web-flux:
            enabled: true
            extended-logging-enabled: true

    ```
    ````
* Исхаков Фан Радисович @fiskhakov17:33у тебя реактивный микросервис?
* Лавриненко Дмитрий Евгеньевич @delavrinenko17:33Да
*   Если отключить либу с логгированием ошибки логгирутся как надо\


    ````
    ```
    2023-07-13 17:31:00.343  WARN [corp-sbp-b2c-api,d0a2ce5cc319135a,d0a2ce5cc319135a] 34919 --- [ctor-http-nio-4] r.a.e.c.model.IntermediateErrorContext   : An error occurred due to a client request.
    code - COMMON
    type - LIMIT_EXCEEDED
    exception - ru.alfabank.corp.api.sbp.exception.LimitExceededException: Превышен общий лимит

    ```
    ````
* Исхаков Фан Радисович @fiskhakov17:34[https://git.moscow.alfaintra.net/projects/CORP-SHOWCASE/repos/corp-advertisement-api/browse/src/main/resources/config/application.yml#83](https://git.moscow.alfaintra.net/projects/CORP-SHOWCASE/repos/corp-advertisement-api/browse/src/main/resources/config/application.yml#83)
* Лавриненко Дмитрий Евгеньевич @delavrinenko17:38А не, даже с либой нужный лог есть\
  Но пустой лог ответа все равно некрасиво
* Исхаков Фан Радисович @fiskhakov17:39там проблема в самой библиотеке
* последняя у тебя версия?
* есть фикс fix: исправлена ошибка, когда ошибочные запросы не логировались, когда был указан уровень логирования WARN и ERROR
* последняя 3.2.35\
  [https://git.moscow.alfaintra.net/projects/TEC/repos/spring-boot-starter-alfalab-logging/commits](https://git.moscow.alfaintra.net/projects/TEC/repos/spring-boot-starter-alfalab-logging/commits)
* Лавриненко Дмитрий Евгеньевич @delavrinenko17:44Спасибо, сейчас попробую\
  стартером тянулась 34-я
