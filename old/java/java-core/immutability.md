# Immutability

Disadvantages

* Mutable objects allows mutating throughout methods parameters
* void return type and mutate inside - a design problem
* Allow to distribute and complicate object creation throughout code
* Don't well in multithreading - possibility for race conditions

\
How to make immutable

* Make all fields final
* Define constructor
* replace setters
* Use @Wither instead of @Setter

\
Immutable collections

* Without adding\deleting\truncating
* All collection wrappers have to implement modification methods, but they can throw exceptions here.
* Collections::unmodifiableList\Set\Map - throw UnsupportedOperationException
