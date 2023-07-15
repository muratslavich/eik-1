# Extension functions



## run, with, let, also and apply

Differences is scope of calling.

run

```
public inline fun <R> run(block: () -> R): R { }
```

```
fun test() {
 var mood = "I am sad"
 run {
   val mood = "I am happy"
   println(mood) // I am happy
 }

 println(mood) // I am sad
}
```

```
run {
  (firstTimeView) introView else normalView
}.show()
```

block run return result type

```
private fun String.show() {
    println(this)
}
```

here **this** is object which called extension function

```
webview.settings.run {
  javaScriptEnabled = true
  databaseEnabled = true
}
```

extension run with this scope

with

```
public inline fun <T, R> with(receiver: T, block: T.() -> R): R {
  return receiver.block()
}
```

```
with(webview.settings) {
  javaScriptEnabled = true
  databaseEnabled = true
}
```

_**block**_ will be executed on **receiver** scope

```
// Yack!
with(webview.settings) {
      this?.javaScriptEnabled = true
      this?.databaseEnabled = true
}
// Nice.
webview.settings?.run {
    javaScriptEnabled = true
    databaseEnabled = true
}
```

let

```
public inline fun <T, R> T.let(block: (T) -> R): R {
    return block(this)
}
```

T.let is sending itself into the function i.e. block: (T)

Calls the specified function \[block] with \`this\` value as its argument and returns its result.

```
stringVariable?.run {
      println("The length of this String is $length")
}

// Similarly.

stringVariable?.let {
      println("The length of this String is ${it.length}")
}

stringVariable?.let {
     nonNullString ->
     println("The non null string is $nonNullString")
}
```

also

```
public inline fun <T> T.also(block: (T) -> Unit): T {
    block(this)
    return this
}
```

```
stringVariable?.let {
      println("The length of this String is ${it.length}")
}

// Exactly the same as below

stringVariable?.also {
      println("The length of this String is ${it.length}")
}
```

T.let returns a different type of value

**T.also returns this itself**

```
settings.also {
   it.javaScriptEnabled = true
   it.databaseEnabled = true
}.check()
```

chain of let and also

```
 val original = "abc"
    // Evolve the value and send to the next chain
    original.let {
        println("The original String is $it") // "abc"
        it.reversed() // evolve it as parameter to send to next let
    }.let {
        println("The reverse String is $it") // "cba"
        it.length  // can be evolve to other type
    }.let {
        println("The length of the String is $it") // 3
    }

    // Wrong
    // Same value is sent in the chain (printed answer is wrong)
    original.also {
        println("The original String is $it") // "abc"
        it.reversed() // even if we evolve it, it is useless
    }.also {
        println("The reverse String is $it") // "abc"
        it.length  // even if we evolve it, it is useless
    }.also {
        println("The length of the String is $it") // "abc"
    }

    // Corrected for also (i.e. manipulate as original string
    // Same value is sent in the chain
    original.also {
        println("The original String is $it") // "abc"
    }.also {
        println("The reverse String is ${it.reversed()}") // "cba"
    }.also {
        println("The length of the String is ${it.length}") // 3
    }
```

* It can provide a very clear separation process on the same objects i.e. making smaller functional section.
* It can be very powerful for self manipulation before being used, making a chaining builder operation.

```
// Normal approach
fun makeDir(path: String): File  {
    val result = File(path)
    result.mkdirs()
    return result
}
// Improved approach
fun makeDir(path: String) = path.let{ File(it) }.also{ it.mkdirs() }
```

apply

all together

* It is an extension function
* It send this as it’s argument.
* It returns this (i.e. itself)

```
public inline fun <T> T.apply(block: T.() -> Unit): T {
    block()
    return this
}
```

```
// Normal approach
fun createInstance(args: Bundle) : MyFragment {
    val fragment = MyFragment()
    fragment.arguments = args
    return fragment
}
// Improved approach
fun createInstance(args: Bundle) 
              = MyFragment().apply { arguments = args }
```

```
// Normal approach
fun createIntent(intentData: String, intentAction: String): Intent {
    val intent = Intent()
    intent.action = intentAction
    intent.data = Uri.parse(intentData)
    return intent
}
// Improved approach, chaining
fun createIntent(intentData: String, intentAction: String) =
        Intent().apply { action = intentAction }
                .apply { data = Uri.parse(intentData) }
```

![](https://miro.medium.com/max/761/1\*pLNnrvgvmG6Mdi0Yw3mdPQ.png)
