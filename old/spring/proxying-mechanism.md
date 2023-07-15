# Proxying mechanism

* Dynamic proxy
* CGLib

![](https://miro.medium.com/max/1919/0\*NCwXFZjXQUlLdLWb.png)

* If the target object to be proxied implements
* **All of the interfaces**
* If the target object

If you want to force the use of **CGLIB** proxying:

* To force the use of CGLIB proxies set the value of the proxy-target-class attribute of the \<aop:config> element to true
* To force CGLIB proxying when using the
* **final methods**
* You will need the CGLIB 2 binaries on your classpath, whereas dynamic proxies are available with the JDK.
* The





self-invocation - invocation inside object with this reference. It means that self-invocation is _**not**_** going to result in the advice** associated with a method invocation

```
public class SimplePojo implements Pojo {

   public void foo() {
      // this next method invocation is a direct call on the 'this' reference
      this.bar();
   }
   
   public void bar() {
      // some logic...
   }
}
```

* The best approach - to refactor your code such that the self-invocation does not happen.
* Use bean from context instead of this invocation

```
public class SimplePojo implements Pojo {

   public void foo() {
      // this works, but... gah!
      ((Pojo) AopContext.currentProxy()).bar();
   }
   
   public void bar() {
      // some logic...
   }
}
```
