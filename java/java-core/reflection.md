# Reflection

````

```
Field[] fields = person.getClass().getDeclaredFields();
fields[0].getName();
List<String> actualFields = getFieldNames(fields);
Field field = person.getClass().getDeclaredField("walks");
Class<?> fieldClass = field.getType();
field.setAccessible(true);
field.set(bird, true);

//get class object
Class<?> clazz = goat.getClass();
Class<?> clazz = Class.forName("com.baeldung.reflection.Goat");

//get name
clazz.getName();
clazz.getCanonicalName();

//get modifier
int goatMods = clazz.getModifiers();
Modifier.isPublic(goatMods);

//get package
Package pkg = clazz.getPackage();
pkg.getName();

//get superclass
Class<?> goatSuperClass = clazz.getSuperclass();

//get interfaces
Class<?>[] goatInterfaces = clazz.getInterfaces();

//get constructors
Constructor<?>[] constructors = clazz.getConstructors();
Constructor<?> cons1 = clazz.getConstructor(String.class);
Bird bird1 = (Bird) cons1.newInstance();

//get methods
Method[] methods = clazz.getDeclaredMethods();
List<String> actualMethods = getMethodNames(methods);
```

````
