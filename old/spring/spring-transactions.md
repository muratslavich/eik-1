# Spring transactions

Software-based transaction management

* declare DataSourceTransactionManager bean (takes dataSource) in config
* use TransactionTemplate bean to manage transactions in code

```
private final TransactionTemplate transactionTemplate;

public ServiceImpl(PlatformTransactionManager transactionManager) {    
    this.transactionTemplate = new TransactionTemplate(transactionManager);  
}

// set parameters
transactionTemplate.setIsolationLevel(TransactionDefinition.ISOLATION_READ_UNCOMMITTED);

transactionTemplate.execute(new TransactionCallback() {
    // do transaction logic
})
```

Declarative transaactions

* declare DataSourceTransactionManager bean (takes dataSource) in config
* add annotation support by
* use @Transactional

@Transactional on interface or classes doesn't work with proxying mechanism.

Internal method call doesn't be transnational.

[Isolation level](https://www.evernote.com/shard/s696/nl/1/2c973536-4148-413c-94c3-f1f614e7bef1?title=Transactions) **@Transactional (isolation=Isolation.READ\_COMMITTED)**

* **DEFAULT**
* **READ\_COMMITED**
* **READ\_UNCOMMITED**
* **SERIALIZABLE**

Transaction propagation **@Transactional(propagation=Propagation.REQUIRED)**

* **REQUIRED**
* **REQUIRES\_NEW -**
* **MANDATORY -**
* **SUPPORTS -**
* **NOT\_SUPPORTED -**
* **NEVER -**
* **NESTED -**
