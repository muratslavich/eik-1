# Generics

Животное <- КошкаЖивотное особь = new Кошка()

| Ковариантность    | _Множество<Животные>  = Множество<Кошки>_                                           |
| ----------------- | ----------------------------------------------------------------------------------- |
| Контрвариантность | _Множество<Кошки> = Множество<Животные>_                                            |
| Инвариантность    | _Множество<Кошки>_ ≠ _Множество<Животные>Множество<Животные>_  ≠ _Множество<Кошки>_ |



Arrays in Java are covariant

```
String[] strings = new String[] {"a", "b", "c"};
Object[] arr = strings;

arr[0] = 42; // ArrayStoreException. Проблема обнаружилась на этапе выполнения программы
```

Generics are invariant

```
List<Integer> ints = Arrays.asList(1,2,3);
List<Number> nums = ints; // compile-time error. Проблема обнаружилась на этапе компиляции
```



Wildcards extends - ограничен сверху super - ограничен снизу

```
// Covariance - Number <- Integer
List<Integer> ints = new ArrayList<Integer>();
List<? extends Number> nums = ints;

// Contravariance
List<Number> nums = new ArrayList<Number>();
List<? super Integer> ints = nums;
```

#### PECS (Producer Extends Consumer Super)

```
public static <T> void copy(List<? super T> consumer, List<? extends T> producer) {
…
}

List<Number> nums = Arrays.<Number>asList(4.1F, 0.2F);
List<Integer> ints = Arrays.asList(1,2);
Collections.copy(nums, ints);
Collections.copy(ints, nums); // Compile-time error
```

#### \<?> и Raw типы

**?** равносилен **? extends Object**, а значит — коллекция может содержать объекты любого класса, так как все классы в Java наследуются от Object – поэтому подстановка называется неограниченной.Используя Raw типы, мы возвращаемся в эру до дженериков и сознательно отказываемся от всех фич, присущих параметризованным типам.

ArrayList arrayList = new ArrayList();

#### Multiple bounds (множественные ограничения)

`<T extends Object & Comparable<? super T>> T max(Collection<? extends T> coll)`

Левая граница используется для затирания, правая для проверок.

#### Type Erasure

| **T (Тип)**                                           | **\|T\| (Затирание типа)** |
| ----------------------------------------------------- | -------------------------- |
| _List< Integer>, List< String>, List< List< String>>_ | _List_                     |
| _List< Integer>\[]_                                   | _List\[]_                  |
| _List_                                                | _List_                     |
| _int_                                                 | _int_                      |
| _Integer_                                             | _Integer_                  |
| _\<T extends Comparable\<T>>_                         | _Comparable_               |
| _\<T extends Object & Comparable\<? super T>>_        | _Object_                   |
| _LinkedCollection\<E>.Node_                           | _LinkedCollection.Node_    |

#### Heap Pollution

```
static List<String> t() {
   List l = new ArrayList<Number>();
   l.add(1);
   List<String> ls = l; // Unchecked assignment
   ls.add("");
   return ls;
}
```



* Все reifiable доступны через механизм Reflection
* Информация о типе полей класса, параметров методов и возвращаемых ими значений доступна через Reflection.

java.lang.reflect.Method.getGenericReturnType()
