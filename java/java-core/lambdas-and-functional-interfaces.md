# Lambdas and functional interfaces

**Functional interface** - any interface with **single abstract method**, and its implementation may be treated as **lambda** expressions.It can have multiple **default methods**.\
default methods

```
interface Human {
    default void walk() {
        System.out.println("Ну, я пошел...");
    }
}

Human.walk(); //ошибка, так делать нельзя

Human human = new Human() {};
human.walk(); //а вот так можно
```

* Default methods can be overridden

```
class Runner implements Human {
    @Override
    public void walk() {
        System.out.println("Я бегу");
    }
}
```

* Can not implements multiple interface with default methods
* In interface we can declare static methods
* Static methods doesn't  inherited

```
interface Human {
    static void walk() {
        System.out.println("Ну, я пошел...");
    }
}

Human.walk();

class HomoSapiens implements Human {}
HomoSapiens.walk(); // compilation error
```

\
Lambda - implementation of abstract method defined in the functional interface.Anonymous implementation of anonymous class related with functional interface.

![](https://2.bp.blogspot.com/-BxiAtQEbcBE/U8fX-k54krI/AAAAAAAAAR4/ke6Ccy4xf0Y/s4000/Java+8+Functional+Interface+Naming+Guide.png)
