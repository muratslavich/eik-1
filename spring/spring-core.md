# Spring core

![2. Introduction to Spring Framework](https://docs.spring.io/spring/docs/4.0.x/spring-framework-reference/html/images/spring-overview.png)[720 × 540](https://www.google.com/url?sa=i\&url=https%3A%2F%2Fdocs.spring.io%2Fspring%2Fdocs%2F4.0.x%2Fspring-framework-reference%2Fhtml%2Foverview.html\&psig=AOvVaw0hfJ0yGEVrn7hUXVcHd-Ml\&ust=1585844170420000\&source=images\&cd=vfe\&ved=0CAIQjRxqFwoTCIjFsobQx-gCFQAAAAAdAAAAABAH)

```
compile("org.springframework:spring-context:5.2.1.RELEASE")

<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context-support</artifactId>
    <version>4.2.2.RELEASE</version>
</dependency>
```

Container

* IoC / DI
* instantiating, configuring, assembling beans by conf metadata
* configuration - XML / annotations / Java code

BeanFactory and ApplicationContext - provides configuration framework and basic functionality

XML configuration

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <import resource="services.xml"/>
    <import resource="resources/messageSource.xml"/>
    <import resource="/resources/themeSource.xml"/>

    <bean id="..." class="..."> 
        <!-- collaborators and configuration for this bean go here -->
        <property name="accountDao" ref="accountDao"/>
        <property name="itemDao" ref="itemDao"/>
       
        <!-- it possible to declare collection in beans>
        <property name = "addressList">
        <list>
            <value>INDIA</value>
            <value>Pakistan</value>
            <value>USA</value>
            <value>USA</value>
        </list>
        </property>

        <!-- it possible to declare inner beans>
        <property name = "target">
          <bean id = "innerBean" class = "..."/>
        </property>
       
        <!-- additional collaborators and configuration for this bean go here -->
    </bean>

    <bean id="..." class="...">
        <!-- collaborators and configuration for this bean go here -->
    </bean>

    <!-- more bean definitions go here -->

</beans>

The id attribute is a string that identifies the individual bean definition.
The class attribute defines the type of the bean and uses the fully qualified classname.
```

Groovy Bean Definition DSL

```
beans {
    dataSource(BasicDataSource) {
        driverClassName = "org.hsqldb.jdbcDriver"
        url = "jdbc:hsqldb:mem:grailsDB"
        username = "sa"
        password = ""
        settings = [mynew:"setting"]
    }
    sessionFactory(SessionFactory) {
        dataSource = dataSource
    }
    myService(MyService) {
        nestedBean = { AnotherBean bean ->
            dataSource = dataSource
        }
    }
}
```

Using container

Instanting container

```
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");
ApplicationContext context = new GenericGroovyApplicationContext("services.groovy", "daos.groovy"); // for Groovy

// retrieve configured instance
PetStoreService service = context.getBean("petStore", PetStoreService.class);
```

\---------------------------------------------------------------------------------------------

@RequestMapping("/path")

@RequestParam(values="name", defaultValue="default") String param

@RestController = @Controller + @ResponseBody

Response convert to JSON by default Jackson in SpringBoot.

@SpringBootApplication

* @Configuration
* @EnableAutoConfiguration
* @ComponentScan

```
./gradlew bootRun

./mvnw spring-boot:run

./gradlew build
java -jar build/libs/gs-rest-service-0.1.0.jar
```



## ApplicationContext

![null](https://www.evernote.com/shard/s696/res/bcec4009-77ea-4759-a7ad-facff3a6899e)

```
String[] beans = context.getBeanDefinitionNames();

// xml configuration
ApplicationContext ctx = new ClassPathXmlApplicationContext("ch/frankel/blog/context.xml");
ApplicationContext ctx = new FileSystemXmlApplicationContext("/opt/app/context.xml");
ApplicationContext ctx = new GenericXmlApplicationContext("classpath:ch/frankel/blog/context.xml");

// self annotated @Component configuration
ApplicationContext ctx = new AnnotationConfigApplicationContext("edu.component");
    0 = "org.springframework.context.annotation.internalConfigurationAnnotationProcessor"
    1 = "org.springframework.context.annotation.internalAutowiredAnnotationProcessor"
    2 = "org.springframework.context.annotation.internalCommonAnnotationProcessor"
    3 = "org.springframework.context.event.internalEventListenerProcessor"
    4 = "org.springframework.context.event.internalEventListenerFactory"
    5 = "foo"

// Java configuration @Configuration which consist methods @Bean return beans
ApplicationContext java = new AnnotationConfigApplicationContext("edu.component");
```

## Spring lifecycle

Spring provides 2 types of container:

* BeanFactory
* ApplicationContext

BeanFactory simplest container provide basic DI support. ApplicationContext extend BeanFactory functionality.

## BeanDefenition

| Sr.No.                   | Properties & Description                                                                                                                                            |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| class                    | This attribute is mandatory and specifies the bean class to be used to create the bean.                                                                             |
| name                     | This attribute specifies the bean identifier uniquely. In XMLbased configuration metadata, you use the id and/or name attributes to specify the bean identifier(s). |
| scope                    | This attribute specifies the scope of the objects created from a particular bean definition and it will be discussed in bean scopes chapter.                        |
| constructor-arg          | This is used to inject the dependencies and will be discussed in subsequent chapters.                                                                               |
| properties               | This is used to inject the dependencies and will be discussed in subsequent chapters.                                                                               |
| autowiring mode          | This is used to inject the dependencies and will be discussed in subsequent chapters.                                                                               |
| lazy-initialization mode | A lazy-initialized bean tells the IoC container to create a bean instance when it is first requested, rather than at the startup.                                   |
| initialization method    | A callback to be called just after all necessary properties on the bean have been set by the container. It will be discussed in bean life cycle chapter.            |
| destruction method       | A callback to be used when the container containing the bean is destroyed. It will be discussed in bean life cycle chapter.                                         |

## BeanScopes

| Sr.No.         | Scope & Description                                                                                                         |
| -------------- | --------------------------------------------------------------------------------------------------------------------------- |
| singleton      | This scopes the bean definition to a single instance per Spring IoC container (default).                                    |
| prototype      | This scopes a single bean definition to have any number of object instances.                                                |
| request        | This scopes a bean definition to an HTTP request. Only valid in the context of a web-aware Spring ApplicationContext.       |
| session        |       This scopes a bean definition to an HTTP session. Only valid in the context of a web-aware Spring ApplicationContext. |
| global-session | This scopes a bean definition to a global HTTP session. Only valid in the context of a web-aware Spring ApplicationContext. |

## Bean lifecycle

Beans have init and destroy methods.&#x20;

```
<bean id = "helloWorld" class = "edu.HelloWorld" init-method = "init" destroy-method = "destroy">
      <property name = "message" value = "Hello World!"/>
</bean>
```

&#x20;BeanPostProcessor interface defines callback methods that you can implement to provide your own instantiation logic

```
public class InitHelloWorld implements BeanPostProcessor {
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        log.info("phase 1");
        return bean;
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        log.info("phase 2");
        return bean;
    }
}

// needs to be declared by bean
<bean class="edu.InitHelloWorld" />
```

Bean initialization phases:

1. constructor
2. postProcessBeforeInitialization
3. init-method
4. postProcessAfterInitialization
5. postProcessBeforeInitialization
6. postProcessAfterInitialization

## Spring Event model in ApplicationContext

* ApplicationEvent - class who provides events
* ApplicationListener - interface who notify implementation about context event

| Sr.No.                | Spring Built-in Events & Description                                                   |
| --------------------- | -------------------------------------------------------------------------------------- |
| ContextRefreshedEvent | This event is published when the                                                       |
| ContextStartedEvent   | This event is published when the                                                       |
| ContextStoppedEvent   | This event is published when the                                                       |
| ContextClosedEvent    | This event is published when the                                                       |
| RequestHandledEvent   | This is a web-specific event telling all beans that an HTTP request has been serviced. |

```
ConfigurableApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
context.start(); // produce ContextStartedEvent

public class ContextStartEventHandler implements ApplicationListener<ContextStartedEvent> {
    @Override
    public void onApplicationEvent(ContextStartedEvent event) {
        log.info("Context started");
    }
}

<bean class="edu.ContextStartEventHandler" />
```

Custom events

```
public class CustomEvent extends ApplicationEvent {
    public CustomEvent(Object source) {
        super(source);
        log.info("custom event constructed");
    }
}

public class CustomEventPublisher implements ApplicationEventPublisherAware {
    private ApplicationEventPublisher applicationEventPublisher;

    @Override
    public void setApplicationEventPublisher(ApplicationEventPublisher applicationEventPublisher) {
        this.applicationEventPublisher = applicationEventPublisher;
    }

    public void publish() {
        CustomEvent customEvent = new CustomEvent(this);
        applicationEventPublisher.publishEvent(customEvent);
    }
}

public class CustomEventHandler implements ApplicationListener<CustomEvent> {
    @Override
    public void onApplicationEvent(CustomEvent event) {
        log.info("Custom event handled");
    }
}

// bean declaration
<bean class="edu.customevent.CustomEventHandler" />
<bean class="edu.customevent.CustomEventPublisher" />

// client code
CustomEventPublisher cep = context.getBean(CustomEventPublisher.class);
cep.publish();
cep.publish();
```

## Mixing different configurations

Need inject one type of configuration to another:

* Java config to xml
* Xml to Java configuration

```
<!-- loading java config files in package -->
<context:component-scan base-package="edu.config" />

<!-- loading additional xml config -->
<import resource="classpath:db-config.xml"/>

@Configuration
@Import({ DbConfig.class })
@ImportResource("classpath:app-config.xml")
public class AppConfig {

}
```
