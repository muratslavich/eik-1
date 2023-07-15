# Syntax



## Language

${variable.name}

$variable

```
"Hello, ${args[0]}!"
```

```
for (item in someCollection)
    println(item)
```

```
val language = if (args.size == 0) "EN" else args[0]
```

```
when (language) {
        "EN" -> "Hello!"
        "FR" -> "Salut!"
        "IT" -> "Ciao!"
        else -> "Sorry, I can't greet you in $language yet"
}

when (obj) {
        1 -> println("One")
        "Hello" -> println("Greeting")
        is Long -> println("Long")
        !is String -> println("Not a string")
        else -> println("Unknown")
}
```

```
fun max(a: Int, b: Int) = if (a > b) a else b
```

```
try {
    return str.toInt()
} catch (e: NumberFormatException) {
    println("One of the arguments isn't Int")
}
```

Any - instead of Object

```
fun getStringLength(obj: Any): Int? {
    if (obj is String)
        return obj.length // no cast to String is needed
    return null
}
```

check range

```
if (x in 1..y - 1)
    println("OK")

if (x !in 1..y - 1)
    println("not OK")
```

iterate over range

```
for (a in 1..5)
    print("${a} ")
```

contain check

```
if ("aaa" in array) // collection.contains(obj) is called
    print("yes")
else print("no")
```

destructing

```
val pair = Pair(1, "one")
val (num, name) = pair

// It required number of component functions can be called on it
class Pair<K, V>(val first: K, val second: V) {
    operator fun component1(): K { return first }
    operator fun component2(): K { return second }
```

data class - toString(), equals(), hashCode() and copy(), component functions

```
data class User(val name: String, val id: Int)
```

hashMap

```
val map = hashMapOf<String, Int>()
map.put("one", 1)
map.put("two", 2)

for ((key, value) in map) {
    println("key = $key, value = $value")
}
```

typealias - provide alternative names for existing types

```
typealias NodeSet = Set<Network.Node>
typealias FileTable<K> = MutableMap<K, MutableList<File>>

typealias MyHandler = (Int, String, Any) -> Unit
typealias Predicate<T> = (T) -> Boolean

typealias AInner = A.Inner
```



### Delegate

get() and set() property methods will be delegated

```
class Example {
    var p: String by Delegate()

    override fun toString() = "Example Class"
}

class Delegate() {
    operator fun getValue(thisRef: Any?, prop: KProperty<*>): String {
        return "$thisRef, thank you for delegating '${prop.name}' to me!"
    }

    operator fun setValue(thisRef: Any?, prop: KProperty<*>, value: String) {
        println("$value has been assigned to ${prop.name} in $thisRef")
    }
}

fun main(args: Array<String>) {
    val e = Example()
    println(e.p)
    e.p = "NEW"
}

// Example Class, thank you for delegating 'p' to me!
// NEW has been assigned to p in Example Class
```

Lazy property

* Delegates.lazy() is a function that returns a delegate that implements a lazy property
* First call to get() executes the lambda expression passed to lazy() as an argument and remembers the result.
* If you want thread safety, use blockingLazy() instead: it guarantees that the values will be computed only in one thread, and that all threads will see the same value.

```
class LazySample {
    val lazy: String by lazy {
        println("computed!")
        "my lazy"
    }
}

fun main(args: Array<String>) {
    val sample = LazySample()
    println("lazy = ${sample.lazy}")
    println("lazy = ${sample.lazy}")
}

// computed!
// lazy = my lazy
// lazy = my lazy
```

* The observable() function takes two arguments: initial value and a handler for modifications
* The handler has 3 parameters: a property being assigned to, the old value and the new one.
* If you want to be able to veto the assignment, use vetoable() instead of observable().

```
class User {
    var name: String by Delegates.observable("no name") {
        d, old, new ->
        println("$old - $new")
    }
}

fun main(args: Array<String>) {
    val user = User()
    user.name = "Carl"
}

// no name - Carl
```

notNull delegate

* you have a non-null var, but you don't have an appropriate value to assign to it in constructor

```
class User {
    var name: String by Delegates.notNull()

    fun init(name: String) {
        this.name = name
    }
}

fun main(args: Array<String>) {
    val user = User()

    // If you read from this property before writing to it, it throws an exception
    
    // user.name -> IllegalStateException
    user.init("Carl")
    println(user.name)
}
```

delgfate can take values from map

```
class User(val map: Map<String, Any?>) {
    val name: String by map
    val age: Int     by map
}

fun main(args: Array<String>) {
    val user = User(mapOf(
            "name" to "John Doe",
            "age"  to 25
    ))

    println("name = ${user.name}, age = ${user.age}")
}
```

#### Delegation pattern

* A class Derived can implement an interface Base by delegating all of its public members to a specified object
* b will be stored internally in objects of Derived and the compiler will generate all the methods of Base that forward to b

```
interface Base {
    fun print()
}

class BaseImpl(val x: Int) : Base {
    override fun print() { print(x) }
}

class Derived(b: Base) : Base by b

fun main() {
    val b = BaseImpl(10)
    Derived(b).print()
}
```



pass function as argument

```
fun main(args: Array<String>) {
    val numbers = listOf(1, 2, 3)
    println(numbers.filter(::isOdd))
}

fun isOdd(x: Int) = x % 2 != 0
```

The composition function return a composition of two functions passed to it:

* compose(f, g) = f(g(\*))

```
fun main(args: Array<String>) {
    val oddLength = compose(::isOdd, ::length)
    val strings = listOf("a", "ab", "abc")
    println(strings.filter(oddLength))
}

fun isOdd(x: Int) = x % 2 != 0
fun length(s: String) = s.length

fun <A, B, C> compose(f: (B) -> C, g: (A) -> B) : (A) -> C {
    return { x -> f(g(x)) }
}
```



### Functions

* can be stored in variables and data structures,
* passed as arguments to
* returned from other

Default argument

```
fun read(b: Array<Byte>, off: Int = 0, len: Int = b.size) { /*...*/ }
```

Named argument

```
fun foo(vararg strings: String) { /*...*/ }

foo(strings = *arrayOf("a", "b", "c"))
```

Unit = void functions

```
fun printHello(name: String?): Unit {
    if (name != null)
        println("Hello ${name}")
    else
        println("Hi there!")
    // `return Unit` or `return` is optional
}

// The Unit return type declaration is also optional

fun printHello(name: String?) { ... }
```

Hugh order functions

* is a function that takes functions as parameters, or returns a function

```
fun <T, R> Collection<T>.fold(
    initial: R, 
    combine: (acc: R, nextElement: T) -> R
): R {
    var accumulator: R = initial
    for (element: T in this) {
        accumulator = combine(accumulator, element)
    }
    return accumulator
}

val joinedToString = items.fold("Elements:", { acc, i -> acc + " " + i })
val product = items.fold(1, Int::times)
```

* parameter combine has a
* To specify that a function type is
* (Int) -> ((Int) -> Unit)

Function type

```
val repeatFun: String.(Int) -> String = { times -> this.repeat(times) }
val twoParameters: (String, Int) -> String = repeatFun

fun runTransformation(f: (String, Int) -> String): String { // type is - (String, Int) -> String
    return f("hello", 3)
}
val result = runTransformation(repeatFun)
```

Obtain an instance of a function type

* lambda -
* anonymous function -
* Using instances of a custom class that implements a function type as an interface:

```
class IntTransformer: (Int) -> Int {
    override operator fun invoke(x: Int): Int = TODO()
}

val intFunction: (Int) -> Int = IntTransformer()
```

Invoking

* f.invoke(x)

Receiver type - A.(B) -> C

```
val intPlus: Int.(Int) -> Int = Int::plus
println(2.intPlus(3))

val sum: Int.(Int) -> Int = { other -> plus(other) }

val sum = fun Int.(other: Int): Int = this + other
```

Passing function in last parameter

```
val product = items.fold(1) { acc, e -> acc * e }
```

If the lambda is the only argument to that call, the parentheses can be omitted entirely

```
run { println("...") }
```

it: implicit name of a single parameter

```
ints.filter { it > 0 }
```

If the lambda parameter is unused, you can place an underscore instead of its name:

```
map.forEach { _, value -> println("$value!") }
```

if you do need to specify it explicitly, you can use an alternative syntax: an _anonymous function_

```
fun(x: Int, y: Int): Int = x + y
```
