# JS syntax



Topic: JS

Course: core

Date: 15.12.2018

Professor:



| Coments |
| ------- |
| \* //   |

* /\* ... \*/

|

| Variables     |
| ------------- |
| \* let x = 5; |

* x = "Bar";
* const y = "Foo";

5 primitive types

* x
* x
* x
* x
* x
* Infinity - бесконечность
* NaN - ошибочный результат числовой операции

Variables can store any value type|

| Simple functions for interactions with user                         |
| ------------------------------------------------------------------- |
| \* prompt(message, default) - will return null if user click Cancel |

* confirm(message) - return true or false
* alert(message) - display message

This functions are modal, dont allow user to interract with  the page before answer |

|                                            | Statements                                                                                     |
| ------------------------------------------ | ---------------------------------------------------------------------------------------------- |
| +                                          | addition. If at least one argument is a string, all arguments cast to the string               |
| ===                                        | accurate compare. Compare includes types comparing                                             |
| ==, <, <=, >, >=                           | Strings are compare lexicographically by unicode.  А < АА < ААА < ААБ < ААВ < АБ < Б < … < ЯЯЯ |
| cast type to number. 0 == false, true > 0. |                                                                                                |
|                                            | (null > 0) - false                                                                             |

(null >= 0) - true because null cast to 0

(null == 0) - false because explicitly stated that null == undefined only.

when comparing are larger/smaller null cast to 0, undefined cast to NaN (null == undefined) - true|

```
if (data.length)
// можно представить как
if (data.length > 0)
```

| functions |
| --------- |
|           |

function factorial(x) {

if (x <= 1) return 1;

return x \* factorial(x - 1);

}

// functional expression

var foo = function(x) {}

// will be executed right after declare

var foo = (function(x) {})

// for functional expression function name needs only for internal call&#x20;

// Declaring functional expression creates closure, which is inheriting the current scope.

var foo = function bar(x) { return bar(x+1); }

// function as a argument of other function call

data.sort(function(a, b) { return a - b; })

// arrow functions

(param1, param2, …, paramN) => { statements }

(param1, param2, …, paramN) => expression

singleParam => { statements }

() => expression

params => ({foo: bar})

(param1, param2, ...rest) => { statements }

(param1 = defaultValue1, param2, …, paramN = defaultValueN) => { statements }

var f = (\[a, b] = \[1, 2], {x: c} = {x: a + b}) => a + b + c;

// by function constructor

var multiply = new Function("x", "y", "return x \* y");

// methods

var obj = {

&#x20; foo() {},

&#x20; bar() {}

}; function foo(a) {}|

Arguments are passed to the function by value.

Objects are passed by reference, through this reference changes are visible outside.

```
function myFunc(theObject) {
    theObject.brand = "Toyota";
}

var mycar = {
    brand: "Honda",
    model: "Accord",
    year: 1998
};

console.log(mycar.brand); // Honda
myFunc(mycar);
console.log(mycar.brand); // Toyota
```

