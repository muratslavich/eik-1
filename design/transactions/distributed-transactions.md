# Distributed transactions



\
Problems\
How maintain consistency in distributed database systems?\


**2 phase commit**\
[940 × 725](https://www.google.com/url?sa=i\&url=https%3A%2F%2Fwww.gridgain.com%2Fresources%2Fblog%2Fapache-ignite-transactions-architecture-2-phase-commit-protocol\&psig=AOvVaw2HPiElivYJWNZHa66rBESk\&ust=1587038534163000\&source=images\&cd=vfe\&ved=0CAIQjRxqFwoTCNDZjrex6ugCFQAAAAAdAAAAABAD)

![](https://www.gridgain.com/sites/default/files/inline-images/Figure1\_7.png)

1. prepare - Coordinator inform other nodes about transaction and wait acknowledge from them.
2. commit - the coordinator informs all nodes about the decision to commit the transaction.

\


* after prepare stage coordinator fault and other nodes can't know about what they should do commit or rollback transaction.
* performance of 2pc is falling with nodes raising

\
Optimistic and pessimistic lock

* **optimistic** - when you **check if the record was updated** by someone else **before** you **commit** the transaction. We should to keep version. (timestamp or other)
* **pessimistic** - when you **take** an exclusive **lock** so that no one else can start modifying the record.

\
In JPA optimistic lock exist

`entityManager.lock(student, LockModeType.OPTIMISTIC);`

When we using optimistic lock we believe that the most operations finish well, and only when error occurred we perform actions.\


**Pattern Saga.** Saga is a sequence of local transactions.Each transaction update db and publish message or event to trigger next transaction in the saga.If transaction fails saga executes a series of compensating transactions.\
Way to coordinate saga

* Choreography - each local transaction publishes domain events that trigger transactions in other services
* Orchestration - an orchestrator (object) tells the participants what local transactions to execute

\
Choreography

![](https://chrisrichardson.net/i/sagas/Create\_Order\_Saga.png)

1. The Order Service receives the POST /orders request and creates an Order in a PENDING state
2. Emit an Order Created event
3. The Customer Service’s event handler attempts to reserve credit
4. Emit an event indicating the outcome
5. The OrderService’s event handler either **approves** or **rejects** the Order

\
Orchestration

![](https://chrisrichardson.net/i/sagas/Create\_Order\_Saga\_Orchestration.png)

1. The Order Service **receives** the POST /orders request and **creates** the Create Order saga orchestrator
2. The saga orchestrator **creates** an Order in the PENDING state
3. It then **sends** a Reserve Credit command to the Customer Service
4. The Customer Service **attempts** to reserve credit
5. It then **sends back** a reply message indicating the outcome
6. The saga orchestrator either **approves** or **rejects** the Order

\
Saga steps should be **idempotent** to can return to previous steps. We can repeat any step. For example if we not receive message about successful completion\
Advantages

* Provide consistency of data between services

\
Disadvantages

* Saga isn't provide isolation between transactions. It can cause of anomalies such 'dirty reading', to prevent it we can use versioning mechanism.
* We should think about compensation transactions
