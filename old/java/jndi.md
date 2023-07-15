# JNDI

\
![](http://java-online.ru/images/sql/jndi.png)\
_JNDI_ представляет обобщенную абстракцию доступа к самым разным службам имен, таким как LDAP (Lightweight Directory Access Protocol), DNS (Domain Naming System), NIS (Network Information Service), NDS (Novell Directory Services), RMI (Remote Method Invocation), CORBA (Common Object Request Broker Architecture). Получив экземпляр контекста JNDI, его можно использовать для поиска ресурсов в любой службе имен, доступной этому контексту. За кулисами **JNDI** взаимодействует со всеми доступными ей службами имен, передавая им имена ресурсов, которые требуется найти и выясняя, где в действительности находится искомый ресурс.\


### Инициализация контекста JNDI

````
```
Properties properties = new Properties();

String CONTEXT  = "oracle.j2ee.rmi.RMIInitialContextFactory";
String URL      = "ormi://<host>:<port>/app";
String LOGIN    = "SCOTT";
String PASSWORD = "TIGER";

properties.put(Context.INITIAL_CONTEXT_FACTORY, CONTEXT );
properties.put(Context.PROVIDER_URL           , URL     );
properties.put(Context.SECURITY_PRINCIPAL     , LOGIN   );
properties.put(Context.SECURITY_CREDENTIALS   , PASSWORD);

// При работе в окружении Java EE (WEB), сервер приложений загружает все необходимые библиотеки для доступа к окружению JNDI, иначе их нужно определить в properties/

Context context = new InitialContext(properties);
```

Сервер приложений втоматически будет использовать jndi.properties из classpath.
```
Context context = new InitialContext();
```
````

| Свойство                                                      | Описание                                                                                                         | Пример                                   |
| ------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- | ---------------------------------------- |
| java.naming.factory.initialContext.INITIAL\_CONTEXT\_FACTORY  | Имя фабричного класса, который будет использоваться для создания контекста.                                      | oracle.j2ee.rmi.RMIInitialContextFactory |
| java.naming.provider.urlContext.PROVIDER\_URL                 | Адрес URL службы JNDI - определение расположения службы (формат описания URL зависит от службы).                 | ormi://localhost:98765/chapter1          |
| java.naming.security.principalContext.SECURITY\_PRINCIPAL     | Идентификационная информация (учетная запись), позволяющая аутентифицировать вызывающую программу в службе JNDI. | SCOTT                                    |
| java.naming.security.credentialsContext.SECURITY\_CREDENTIALS | Используемый для аутентификации пароль пользователя.                                                             | TIGER                                    |

### Поиск ресурсов в JNDI, lookup

`BidService service = (BidService) context.lookup("/ejb/bid/BidService");`\
