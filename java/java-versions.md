# Java versions

{% embed url="https://www.marcobehler.com/guides/a-guide-to-java-versions-and-features" %}

![](<.gitbook/assets/image (1).png>)

### Java 8

Lambda

```java
Runnable runnable = () -> System.out.println("Hello world two!");
```



Stream

```java
Arrays.asList("franz", "ferdinand", "fiel", "vom", "pferd")
    .stream()
    .filter(name -> name.startsWith("f"))
    .map(String::toUpperCase)
    .sorted()
    .forEach(System.out::println);
```



### Java 9

Collections helpers

```java
List<String> list = List.of("one", "two", "three");
Set<String> set = Set.of("one", "two", "three");
Map<String, String> map = Map.of("foo", "one", "bar", "two");
```



Stream&#x20;

```java
takeWhile,dropWhile,iterate

Stream<String> stream = Stream.iterate("", s -> s + "s")
  .takeWhile(s 
```



Optional

```java
user.ifPresentOrElse(this::displayAccount, this::displayLogin);
```



jshell

HTTPCLient



### Java 10

keyword var

```java
var myName = "Marco"
```



### Java 11

run files

```
ubuntu@DESKTOP-168M0IF:~$ java MyScript.java
```



Local vars for lambdas

```java
(var firstName, var lastName) -> firstName + lastName
```



### Java 12

Java 12 got a couple [new features and clean-ups](https://www.oracle.com/technetwork/java/javase/12-relnote-issues-5211422.html), but the only ones worth mentioning here



### Java 14

Switch

```java
int numLetters = switch (day) {
    case MONDAY, FRIDAY, SUNDAY -> 6;
    case TUESDAY                -> 7;
    default -> {
        String s = day.toString();
        int result = s.length();
        yield result;
    }
};
```

&#x20;

Record classes which only contains data, (potentially) getters/setters, equals/hashcode, toString.

```java
record Point(int x, int y) { }
```



The Concurrent Mark Sweep (CMS) Garbage Collector has been removed, and the experimental Z Garbage Collector has been added.



### Java 15

Text blocks

```java
String text = """
                Lorem ipsum dolor sit amet, consectetur adipiscing \
                elit, sed do eiusmod tempor incididunt ut labore \
                et dolore magna aliqua.\
                """;
```



Sealed classes

```java
public abstract sealed class Shape
    permits Circle, Rectangle, Square {...}
```



### Java 16

Convenient instanceOf

```java
if (obj instanceof String s) {
    // Let pattern matching do the work!
    // ... s.substring(1)
}
```



### Java 17

Pattern Matching for switch (Preview)

```java
public String test(Object obj) {
    return switch(obj) {
        case Integer i -> "An integer";
        case String s -> "A string";
        case Cat c -> "A Cat";
        default -> "I don't know what it is";
    };

}
```





