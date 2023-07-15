# OOP

### Principles

\
Abstractions - realizing complex thing by abstractions.\


* Encapsulation - keeps realization inside and private, but provide public interface
* Inheritance - deriving some functional from parent class. Child extend base class. Relate as is.
* Polymorphism - many realization of one class can be used in one way by base class.\


Composition relationship - is a part of. When some class manage lifecycle of other and contain in itself. F.e. container and beans.Aggregation - has a relationship. F.e. database and software, teacher and students.

**Composition over inheritance principle -** gives more flexible design. It is more natural to build business-domain classes out of various components than trying to find commonality between them and creating a family tree. And we have not to follow rules of inheritance such as Liskov rule.

**Diamond problem of multiple inheritance**

<div align="left">

<img src="https://javapapers.com/wp-content/uploads/2012/09/Diamond-Problem-of-Multiple-Inheritance.png" alt="">

</div>

We have two classes B and C inheriting from A. Assume that B and C are overriding an inherited method and they provide their own implementation. Now D inherits from both B and C doing multiple inheritance. D should inherit that overridden method, which overridden method will be used? Will it be from B or C? Here we have an ambiguity.\


### Design principles

* SOLID
*
  * Single responsibility
  * Open for extension and close for modification
  * Liskov substitution principle - objects in a program should be replaceable with instances of their subtypes without altering the correctness of that program.
  * Interface segregation principle
  * Dependency inversion principle - High-level modules should not depend on low-level modules. Dependency injection pattern solve this problem.
* DRY - Don\`t repeat yourself
* YAGNI - You ain't gonna need it
* KISS - keep it stupid simple
* GRASP - General Responsibility Assignment Software Patterns
*
  * Controller
  * Creator
  * Indirection
  * Information expert
  * High cohesion and low coupling
  * Polymorphism
  * Protected variations
  * Pure fabrication
* ACID
*
  * Atomic
  * Consistency - any data written to the database must be valid
  * Isolation - ensures that concurrent execution of transactions leaves the database in the same state that would have been obtained if the transactions were executed sequentially
  * Durability - guarantees that once a transaction has been committed, it will remain committed even in the case of a system failure
* CQRS - Command–query separation is a principle of imperative computer programming.

\
**High cohesion and low coupling**

* low coupling - How much do your different modules depend on each other?
* high cohesion - represents the degree to which a part of a code base forms a logically single, atomic unit.

## Design patterns

* Creational
*
  * Factory method
  * Abstract Factory
  * Builder
  * Prototype
* Structural
*
  * Adapter
  * Bridge
  * Composite
  * Decorator
  * Facade
  * Flyweight
  * Proxy
* Behavioral
*
  * Chain of Responsobility
  * Command
  * Iterator
  * Mediator
  * Memento
  * Observer
  * State
  * Strategy
  * Template Method
  * Visitor
* [Concurrency](https://en.wikipedia.org/wiki/Concurrency\_pattern)
*
  * Double-checked locking
  * Monitor Object
  * Read write lock pattern
  * Scheduler pattern
  * Thread pool pattern

\
\
\
