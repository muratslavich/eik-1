# Map

## 9 главных вопросов о Map в Java

[Оригинал](http://www.programcreek.com/2013/09/top-9-questions-for-java-map/)

**Map - структура данных, набор пар ключ-значение, и каждый ключ может использоваться только один раз в одной Map.**

**Обращение Map в List**

```
List keyList = new ArrayList(Map.keySet());
List valueList = new ArrayList(Map.valueSet());
List entryList = new ArrayList(Map.entrySet());
```

**1. Пройтись по всем значениям в Map.**

```
for(Entry entry: Map.entrySet()) {  
    //получить ключ  
    K key = entry.getKey();  
    //получить значение  V value = entry.getValue();
}
```

Так же мы можем использовать Iterator, особенно в версиях младше JDK 1.5

```
Iterator itr = Map.entrySet().iterator();
while(itr.hasNext()) {  
    Entry entry = itr.next();  
    //получить ключ  
    K key = entry.getKey();  
    //получить значение  
    V value = entry.getValue();
}
```

**2. Упорядочивание Map по ключам**

```
List list = new ArrayList(Map.entrySet());
Collections.sort(list, new Comparator() {   
    @Override  public int compare(Entry e1, Entry e2) {    
        return e1.getKey().compareTo(e2.getKey());  
    } 
});
```

Другой способ: **SortedMap —** [**TreeMap**](http://docs.oracle.com/javase/7/docs/api/java/util/TreeMap.html). Её конструктор принимает компаратор.

```
SortedMap sortedMap = new TreeMap(new Comparator() {  
    @Override  public int compare(K k1, K k2) {    
        return k1.compareTo(k2);  
    } 
});
sortedMap.putAll(Map);
```

**3. Упорядочивание Map по значениям**

```
List list = new ArrayList(Map.entrySet());
Collections.sort(list, new Comparator() {   
    @Override  public int compare(Entry e1, Entry e2) {    
        return e1.getValue().compareTo(e2.getValue());  
    } 
});
```

**4. immutable Map**

Для создания неизменяемой Map с использованием статического инициализатора, нам нужен супер анонимный класс, который мы добавим в неизменяемую Map на последнем шаге инициализации.

```
public class Test {   
    private static final Map Map;  
    static {    
        Map aMap = new HashMap();    
        aMap.put(1, "one");    
        aMap.put(2, "two");    
        Map = Collections.unmodifiableMap(aMap);  
    }
}
```

**5. Разница между HashMap, TreeMap, и Hashtable**

* **Порядок прохода**. **HashMap** и **HashTable** нет упорядоченности; TreeMap будет упорядочивать по компаратору.
* **HashMap** позволяет иметь _ключ null_ и _значение null_. **HashTable** не позволяет _ключ null_ или _значение null_. Если **TreeMap** использует естественный порядок или компаратор не позволяет использовать _ключ null_, будет выброшено исключение.
* **HashTable** - синхронизирована

Более подробное сравнение

```
.                      | HashMap | HashTable  | TreeMap-------------------------------------------------------Упорядочивание          |нет      |нет        | даnull в ключ-значение    | да-да   | нет-нет     | нет-дасинхронизировано        | нет     | да        | нетпроизводительность      | O(1)    | O(1)      | O(log n)воплощение             | корзины | корзины   | красно-чёрное дерево
```

**6. Map с реверсивным поиском/просмотром**

Иногда, нам нужен набор пар ключ-ключ, что подразумевает значения так же уникальны, как и ключи (паттерн один-к-одному). Такое постоянство позволяет создать «инвертированный просмотр/поиск» по Map. То есть, мы можем найти ключ по его значению. Такая структура данных называется [**двунаправленная Map**](http://en.wikipedia.org/wiki/Bidirectional\_map), которая к сожалению, не поддерживается JDK.

Обе Apache Common Collections и Guava предлагают воплощение двунаправленной Map, называемые [BidiMap](http://commons.apache.org/proper/commons-collections/javadocs/api-3.2.1/org/apache/commons/collections/BidiMap.html) и [BiMap](http://guava-libraries.googlecode.com/svn/tags/release09/javadoc/com/google/common/collect/BiMap.html), соответственно. Обе вводят ограничение, которое задаёт соответствие 1:1 между ключами и значениями.

**7. Поверхностная копия Map**

Почти все, если не все, Map в Java содержат конструктор копирования другой Map. Но процедура копирования не синхронизирована. Что означает когда один поток копирует Map, другой может изменить её структуру. Для предотвращения внезапной рассинхронизации копирования, один из них должен использовать в таком случае **Collections**_.synchronizedMap()_.

```
Map copiedMap = Collections.synchronizedMap(Map);
```

Другой интересный способ поверхностного копирования — использование метода _clone()_. Но он **НЕ**рекомендуется даже создателем фреймворка коллекций Java, Джоном Блохом. В споре "[Конструктор копирования против клонирования](http://www.artima.com/intv/bloch13.html)", он занимает позицию:

Цитата:> «Я часто привожу публичный метод clone в конкретных классах, поскольку люди ожидают их там увидеть.… это позор, что Клонирование сломано, но это случилось.… Клонирование это слабое место, и я думаю люди должны быть предупреждены о его ограничениях.»

По этой причине, я даже не показываю вам, как использовать метод _clone()_ для копирования Map&#x20;

**8. Создание пустой Map**

Если Map неизменна, используйте

```
Map = Collections.emptyMap();
```

Или, используйте любое другое воплощение. Например

```
Map = new HashMap();
```

КОНЕЦ
