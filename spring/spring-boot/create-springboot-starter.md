# Create SpringBoot starter

Современный Spring, а если быть точнее — специальный фреймворк Spring Boot, позволяют с минимальными усилиями подключать ту или иную технологию. Необходимость создавать десятки служебных бинов ушла в прошлое.

Для этого имеются всевозможные starter-ы — специальные Maven/Gradle зависимости, которые необходимо только подключить в проект.

Например, подключив в проект всего одну зависимость:

```
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-jdbc</artifactId>
    </dependency>
```

мы имеем уже созданный в контексте DataSource, созданный по свойствам в application.properties и другими классами, вроде NamedParameterJdbcTemplate.

Подобные starter основаны на двух специальных функциональностях Spring Boot — AutoConfigurations и Conditional. Покажем, как использовать данные функциональности на примере **создания собственного Spring Boot Starter**.

Для начала представим, что мы работаем в компании “Марсианская Почта” (com.martianpost). В нашей компании написано множество приложений, для простоты, ровно два — консольное app-example и веб-приложение web-app-example.

Создадим эти приложения с помощью [Spring Initializr](https://start.spring.io/).

Исходные коды их можно найти вот [здесь](https://github.com/ydvorzhetskiy/starter-example).

Т. к. мы “Марсианская Почта”, то нам очень важно в каждом приложении знать точное марсианское время (а точнее MSD — Mars Sol Date) для всех приложений.

Выпустим Spring Boot Starter, решающий эту задачу. В соответствии с [документацией](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-custom-starter-naming) выдадим следующие Maven-координаты:

```
    <project ...>
        <groupId>com.martianpost</groupId>
        <artifactId>martian-time-spring-boot-starter</artifactId>
        <version>1.0.0</version>

        ...
    </project>
```

Для больших проектов и технологий, можно вынести отдельно, как саму технологию — martian-time, так и автоконфигурацию — martian-time-autoconfigure и сам стартер — martian-time-spring-boot-starter. Но для педагогических целей мы просто всё напишем в starter-е.

Из зависимостей мы оставим нам необходим только **`spring-boot-starter`**, правда, добавим его с optional-параметром — наш starter не является starter-ом уровнем всего приложения (как, например, spring-boot-starter-web), поэтому **`spring-boot-starter`** starter уже будет добавлен в приложение.

```
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
        <optional>true</optional>
    </dependency>
```

Ну и реализуем наш сервис:

```
    package com.martianpost.martiantime.service;

    import org.springframework.stereotype.Service;

    import java.time.Duration;
    import java.time.ZonedDateTime;

    @Service
    public class MartianTimeService {

        private static final ZonedDateTime MID_DAY
            = ZonedDateTime.parse("2000-01-06T00:00:00Z");

        public double toMarsSolDate(ZonedDateTime zonedDateTime) {
            double secondsFromMidDay
                = (double) Duration.between(MID_DAY, zonedDateTime).getSeconds();
            return secondsFromMidDay / 88775.244 + 44795.9998;
        }
    }
```

Не забудем про тест:

```
    package com.martianpost.martiantime.service;

    ...

    @DisplayName("Сервис MartianTimeService")
    class MartianTimeServiceTest {

        private final MartianTimeService service = new MartianTimeService();

        @DisplayName("должен конвертировать совпадение полночей в 06.01.2000")
        @Test
        void shouldConvertZeroDay() {
            ZonedDateTime zeroDayUtc = ZonedDateTime.parse("2000-01-06T00:00:00Z");
            double result = service.toMarsSolDate(zeroDayUtc);
            assertEquals(44_795.9998, result, 1e-3);
        }

        @DisplayName("Должен конвертировать пример с http://jtauber.github.io/mars-clock/")
        @Test
        void shouldConvertExampleFromGithub() {
            ZonedDateTime time = ZonedDateTime.parse("2020-05-01T09:44:43Z");
            double result = service.toMarsSolDate(time);
            assertEquals(52_018.84093, result, 1e-3);
        }
    }
```

Обратим внимание, что если мы подключим данную библиотеку, то сервис не создастcя автоматически (хотя аннотация @Service) — для этого как раз и нужны автоконфигурации.

Создадим автоконфигурацию для нашего модуля:

```
    package com.martianpost.martiantime;

    ...

    @Configuration
    @ComponentScan
    public class MartianTimeAutoConfiguration {
    }
```

Данный класс ничем не отличается от обычного класса конфигурации. Его главная задача — найти класс, помеченный @Service и создать его бин.

Но кто найдёт этот класс автоконфигурации? Никто, и нужно дополнительно указать эту автоконфигурацию, чтобы Spring Boot нашёл её:

```
    # resources/META_INF/spring.factories
    org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
    com.martianpost.martiantime.MartianTimeAutoConfiguration
```

Да, теперь можно попробовать подключить данный модуль в наше консольное приложение:

```
    <dependency>
        <groupId>com.martianpost</groupId>
        <artifactId>martian-time-spring-boot-starter</artifactId>
        <version>1.0.0</version>
    </dependency>
```

и больше ничего!

```
    @Autowired
    private MartianTimeService martianTimeService;

    @PostConstruct
    public void printCurrentTime() {
        double currentMarsSolDate = martianTimeService.toMarsSolDate(ZonedDateTime.now());
        System.out.println("MSD: " + currentMarsSolDate);
    }
```

И получаем:

```
    MSD: 52018.87995339051
```

Допустим, нам в каждом веб-приложении необходимо сделать, чтобы это время возвращалось RestController-ом. Но проблема в том, что наш стартер может использоваться как в консольных приложениях, так и в веб-приложениях.

ОК, мы это можем сделать с помощью @Conditional. Сначала изменим зависимости нашего starter-а:

```
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <optional>true</optional>
    </dependency>
```

Добавим контроллер, который будет создаваться только в веб-приложениях:

```
    package com.martianpost.martiantime.rest;

    ...
    import org.springframework.boot.autoconfigure.condition.ConditionalOnWebApplication;

    @ConditionalOnWebApplication
    @RestController
    public class MartianTimeController {

        private final MartianTimeService martianTimeService;

        public MartianTimeController(MartianTimeService martianTimeService) {
            this.martianTimeService = martianTimeService;
        }

        @GetMapping("/mds/current")
        public double getMds() {
            return martianTimeService.toMarsSolDate(ZonedDateTime.now());
        }
    }
```

Обратите внимание, что за магию включения/выключения бина отвечает аннотация @ConditionalOnWebApplication. Помимо неё существует множество других @Conditional аннотаций — [ссылка](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-condition-annotations).

Проверим, что наше консольное приложение работает:

```
 MSD: 52018.896467003244
```

Если вывести список зависимостей консольного приложения, то увидим, что spring-mvc там и не присутствует.

А вот подключив в веб-приложение:

```
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
    </dependency>
    <dependency>
        <groupId>com.martianpost</groupId>
        <artifactId>martian-time-spring-boot-starter</artifactId>
        <version>1.0.0</version>
    </dependency>
```

И запустив, мы получим:

```
    GET http://localhost:8080/msd/current
    52018.90259483771
```

Магия :-) Но доступная каждому :-)

Целиком пример Вы можете посмотреть на [GitHub](https://github.com/ydvorzhetskiy/starter-example).
