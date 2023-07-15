# Spring Aop

```
compile("org.springframework:spring-aop:5.2.1.RELEASE")
compile("org.aspectj:aspectjweaver:1.9.5")
```

Examples of aspects:

* logging
* auditing
* transactions
* security
* caching

Termins:

* Aspect - module providing cross-cutting requirenments
* Join point - point where aspect pluged in
* Advice - actual action to be executed before or after
* Pointcut - set of join points where advice should be executed
* Introductions - allows to add new methods or attributes
* Target object - object being adviced by aspect
* Weaving - process of linking aspects

Type of advice:

* before
* after
* after-returning - run advice after a method only if it completes successfully
* after-throwing - only if method exits by throwing an exception
* around - before and after

Aspects implementation

* Xml schema based
* AspectJ

1. create custom annotation

```
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface LogExecutionTime {
 
}
```

1. Creating Our Aspect

```
@Aspect
@Component
public class ExampleAspect {
 
}
```

1. Creating Our Pointcut and Advice

```
@Around("@annotation(LogExecutionTime)")
public Object logExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
    long start = System.currentTimeMillis();
 
    Object proceed = joinPoint.proceed();
 
    long executionTime = System.currentTimeMillis() - start;
 
    System.out.println(joinPoint.getSignature() + " executed in " + executionTime + "ms");
    return proceed;
}
```
