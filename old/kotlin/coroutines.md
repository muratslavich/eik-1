# Coroutines

CoroutineBuilder

* launch {...}  -  launch a new coroutine in background and continue
* runBlocking {...}  - Runs a new coroutine and

CoroutineScope

* GlobalScope

Suspend functions are only allowed to be called from a coroutine or another suspend function.

### Blocking

use runBlocking to wrap the execution of the main function (or in tests)

```
fun main() = runBlocking<Unit> { // start main coroutine
    GlobalScope.launch { // launch a new coroutine in background and continue
        delay(1000L)
        println("World!")
    }
    println("Hello,") // main coroutine continues here immediately
    delay(2000L)      // delaying for 2 seconds to keep JVM alive
}
```

### Waiting for a job

```
val job = GlobalScope.launch { // launch a new coroutine and keep a reference to its Job
    delay(1000L)
    println("World!")
}
println("Hello,")
job.join() // wait until child coroutine completes
```
