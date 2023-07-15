# Context and this

| Passing context to function |
| --------------------------- |
| console.log(this.name)      |

}

let me = {name: "Reader"}

foo.call( me ); // Reader

* call ensure the \`this\`

\--------------------------------

function bar( context ) {

console.log(context.name)

}

bar( me ) // Reader function foo() { | |\* remember

* foo.count
* how store state between function calls?
* _Where better_

function foo( num ) {

console.log( "foo: " + num );

this.count = num;

}

foo.count = 0

foo(3) // foo: 3

foo(5) // foo: 5

console.log( foo.count ) // still 0

* this.count
* this.count created a global variable count.

\| |\* to avoid this problem we can create another object to hold property:

function foo( num ) {

console.log("foo: " + num)

data.count += num;

}

let data = { count: 0 }

foo(3)

foo(5)

console.log( data.count ) // 8 | |\* but you can reference a function object from inside itself by name:

function foo( num ) {

console.log("foo: " + num)

foo.count += num;

}

foo.count = 0

foo(3)

foo(5)

console.log( foo.count )

*   anonymous function cannot refer to itself

    ```
        |
    ```

|var a = 2;

this.bar();

}

function bar() {

console.log( this.a );

}

foo(); //ReferenceError: a is not defined

* It is not possible to create a bridge between the scopes of functions
* there is no bridge

function foo() { | |\* this si actuall a binding references by the call-site where the function is called

```
                                                                                                                                                                                                                                                                                                                                                           |
```

| Default binding            |
| -------------------------- |
| console.log( **this**.a ); |

}

**var** a = 2;

foo(); _// 2_

* var a is declared in global scope
* default binding for this applies this at the global object
* foo() is called undecorated function reference
* strict mode says the global object is not eligible for default binding

**function**|

| Implicit binding           |
| -------------------------- |
| console.log( **this**.a ); |

}

&#x20;  &#x20;

**var** obj = {

a: 2,

foo: foo

};

obj.foo(); _// 2_

foo() // undefined

* function foo conteined by the obj object
* function added as a reference property onto obj
* implicit binding means that object should be used for function call\`s this binding

**function**| |console.log( this.a );

}

var obj = {

a: 2,

foo: foo

};

var bar = obj.foo

var a = "oops, global"

bar() // oops, global

* lose implicit binding and falls back to default
* bar in fact it\`s just another reference to foo itself

function foo() { | |console.log( this.a );

}

function doFoo(fn) {

fn();

}

var obj = {

a: 2,

foo: foo

};

var a = "oops, global"

doFoo( obj.foo ) // "oops, global"

* when passing a callback function it\`s just implicit reference assignment

function foo() { |

| Explicit binding |
| ---------------- |
| \* call( .. )    |

* apply( .. )
* they takes as their first parameter object to use for \`this\`

function foo() {

console.log( this.a );

}

var obj = {

a: 2

};

foo.call( obj ); // 2

*   also can loose binding in callback

    ```
                                                                                                            |
    ```

|console.log( el, this.id );

}

var obj = {

id: "awesome"

};

// use \`obj\` as \`this\` for \`foo(..)\` calls

\[1, 2, 3].forEach( foo, obj );

// 1 awesome 2 awesome 3 awesome

* Internally, these various functions almost certainly use explicit binding
* via call(..) or apply(..), saving you the trouble

function foo(el) {|

| Hard binding           |
| ---------------------- |
| console.log( this.a ); |

}

var obj = {

a: 2

};

var bar = function() {

foo.call( obj );

};

bar(); // 2

setTimeout( bar, 100 ); // 2

// hard-bound \`bar\` can no longer have its \`this\` overridden

bar.call( window ); // 2

* function bar() manually bind this with foo

function foo() { | |console.log( this.a, something );

return this.a + something;

}

var obj = {

a: 2

};

var bar = function() {

return foo.apply( obj, arguments );

};

var b = bar( 3 ); // 2 3

console.log( b ); // 5 function foo(something) { | |console.log( this.a, something );

return this.a + something;

}

// simple \`bind\` helper

function bind(fn, obj) {

return function() {

return fn.apply( obj, arguments );

};

}

var obj = {

a: 2

};

var bar = bind( foo, obj );

var b = bar( 3 ); // 2 3

console.log( b ); // 5 function foo(something) {| |console.log( this.a, something );

return this.a + something;

}

var obj = {

a: 2

};

var bar = foo.bind( obj );

var b = bar( 3 ); // 2 3

console.log( b ); // 5

* Function.prototype.bind returns a new function that is call original with \`this\` context

function foo(something) { |

| new Binding                             |
| --------------------------------------- |
| \* constructor in js is just a function |

```
* new object is created {}
* newly object bind this for function
* return its own alternate object
```

function foo(a) {

this.a = a

}

var bar = new foo(2)

console.log(bar.a) // 2

|

implicit -> explicit/constructor -> hard binding
