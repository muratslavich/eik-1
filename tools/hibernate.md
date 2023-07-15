# Hibernate

### Hibernate

\
![Hibernate Tutorial - HowToDoInJava](https://howtodoinjava.com/wp-content/uploads/2013/02/Hibernate-Architecture.png)\
![Entity life cycle in Hibernate - PrismoSkills](https://prismoskills.appspot.com/lessons/Hibernate/imgs/entity\_life\_cycle.png)

[methods](https://docs.jboss.org/hibernate/orm/3.5/javadocs/org/hibernate/Session.html)



### FetchTypes

* Eager
* Lazy



```
// User
@OneToMany(fetch = FetchType.LAZY, mappedBy = "user")
private Set<OrderDetail> orderDetail = new HashSet();

// Order
@ManyToOne(fetch = FetchType.LAZY)
@JoinColumn(name="USER_ID")
private UserLazy user;
```

Difference - is the moment when data gets loaded into memory\


### N+1

when

````
@ManyToMany(fetch = FetchType.LAZY)                    
private Set<Role> roles;

First Get All User
```
Select * from t_users;
```
Then get roles for each user
```
Select * from t_user_roles where userid = <userid>;
```
````

* join fetch
* plain sql
* using EntityGraph

```
@EntityGraph(attributePaths = {"roles"})                      
List<User> findAll();
```





### Cache

* Кеш первого уровня (First-level cache);
* Кеш второго уровня (Second-level cache);
* Кеш запросов (Query cache);

\
1 level

* binded to session object
* using by default
* can't disable

Один важный момент — при использовании метода load() Hibernate не выгружает из БД данные до тех пор пока они не потребуются.Когда осуществляется первый вызов load, мы получаем прокси объект или сами данные в случае, если данные уже были в кеше сессии.В случае прокси объекта мы можем связать два объекта не делая запрос в базу, в отличии от метода get().При использовании методов save(), update(), saveOrUpdate(), load(), get(), list(), iterate(), scroll() всегда будет задействован кеш первого уровня.\
2 level

* binded to sessionFactory
* disabled by default

```
Session session = factory.openSession();
SharedDoc doc = (SharedDoc) session.load(SharedDoc.class, 1L);
System.out.println(doc.getName());
session.close();

session = factory.openSession();
doc = (SharedDoc) session.load(SharedDoc.class, 1L);   
System.out.println(doc.getName());       
session.close(); 
```



хибернейт сам не реализует кеширование как таковое. А лишь предоставляет структуру для его реализации, поэтому подключить можно любую реализацию, которая соответствует спецификации

* EHCache
* OSCache
* SwarmCache
* JBoss TreeCache

\
Кеш запросов (Query cache);

* disabled by default
* ключом к данным кеша выступает совокупность параметров запроса

\


**Стратегии кеширования**

Стратегии кеширования определяют поведения кеша в определенных ситуациях. Выделяют четыре группы:

* Read-only
* Read-write
* Nonstrict-read-write
* Transactional
