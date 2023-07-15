# Concurrency

## Multithreading

![Multithreading in Java](https://www.w3schools.in/wp-content/uploads/2019/06/Life-Cycle-of-a-Thread-in-Java.png)[790 × 485](https://www.google.com/url?sa=i\&url=https%3A%2F%2Fwww.w3schools.in%2Fjava-tutorial%2Fmultithreading%2F\&psig=AOvVaw0Fuys7KUaoFTs6ZruO593g\&ust=1585673543866000\&source=images\&cd=vfe\&ved=0CAIQjRxqFwoTCPjIxrPUwugCFQAAAAAdAAAAABAI)

How to create thread

* Extending the Thread class
* Implementing the Runnable Interface

```
Thread th = new Thread(runnableThing)
th.start()

th.join() // current thread wait ending th
th.interrupt() // isInterrupted() - true
th.sleep(100ms)
```

**synchronized**

* as mutex of block code
* methods or blocks
* used object monitor this or transferred object

```
semaphore = new Semaphore(slotLimit)
semaphore.tryAcquire()
semaphore.release()
semaphore.availablePermits()
```

**ReentrantLock** ![How will ReentrantLock object created inside a method's local ...](https://i.stack.imgur.com/hyV4N.png)[649 × 184](https://www.google.com/url?sa=i\&url=https%3A%2F%2Fstackoverflow.com%2Fquestions%2F55294843%2Fhow-will-reentrantlock-object-created-inside-a-methods-local-scope-work\&psig=AOvVaw1wgLNeNoCH3dD2P5R3nMex\&ust=1585675144698000\&source=images\&cd=vfe\&ved=0CAIQjRxqFwoTCPi0zL3awugCFQAAAAAdAAAAABAD)

* ReentrantLock is a concrete implementation of Lock interface
* it provides more functionality and differ in certain aspect
* ability to trying for lock interruptibly, and with timeout
* **fairness -**
* ReentrantLock provides convenient
* **ability to interrupt**
* ReentrantLock also provides convenient method to get List of all threads waiting for lock.

**Volatile**

* _Volatile_
* Happens-before guarantee - ensure that values of all the variables including non-volatile variables are written to the main memory along with the

[1137 × 795](https://www.google.com/url?sa=i\&url=https%3A%2F%2Fdevtype.blogspot.com%2F2017%2F05%2FJava-Concurrent-Collections-Diagram-for-study-purposes.html\&psig=AOvVaw0V3fC4\_GRKFmQMonXelavU\&ust=1585679886318000\&source=images\&cd=vfe\&ved=0CAIQjRxqFwoTCND60ITswugCFQAAAAAdAAAAABAD)

![](https://1.bp.blogspot.com/-Emj4Zf4ksXg/WQilwuBa8DI/AAAAAAAAaT8/H3JxhWwQwF0J5HYLXmmnNzlcKgv9gc1TwCLcB/s1600/Java%!B\(MISSING\)8%!B\(MISSING\)ConcurrentCollections.png)
