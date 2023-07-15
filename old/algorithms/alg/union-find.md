# Union-Find

**Union-Find API.**

В качестве базовой структуры данных используется массив индексированный узлами id\[]. Любой компонент представлен одним из своих узлов.

Базовая реализация

public class UF {

&#x20;   private int\[] id;     //&#x20;

&#x20;   private int count;    // number of components

&#x20;   public UF(int n) {       &#x20;

&#x20;       count = n;

&#x20;       id = new int\[n];       &#x20;

&#x20;       for (int i = 0; i < n; i++) {

&#x20;           id\[i] = i;           &#x20;

&#x20;       }

&#x20;   }

&#x20;   public int count() {         return count;    }

&#x20;   public boolean connected(int p, int q) {

&#x20;       return find(p) == find(q);

&#x20;   }

&#x20;   public int find(int p) {     }   &#x20;

&#x20;  &#x20;

&#x20;   public void union(int p, int q) {     }

/\*\*

\* Клиент решает задачу динамической связности. Он читает значение из n, а за ним последовательность

\* пар целых чисел (все из промежутка от 0 до n-1) и для каждой пары вызывает метод find().

\* Если два узла в паре уже соединены, клиент переходит к чтению следующей пары, а если нет, вызывает

\* union() и выводит эту пару. &#x20;

\*/

&#x20;   public static void main(String\[] args) {

&#x20;       int n = StdIn.readInt();

&#x20;       UF uf = new UF(n);

&#x20;       while (!StdIn.isEmpty()) {

&#x20;           int p = StdIn.readInt();

&#x20;           int q = StdIn.readInt();

&#x20;           if (uf.connected(p, q)) continue;

&#x20;           uf.union(p, q);

&#x20;           StdOut.println(p + " " + q);

&#x20;       }

&#x20;       StdOut.println(uf.count() + " components");

&#x20;   }

}

**Быстрый поиск**



Все узлы, принадлежащие какому то компоненту, должны иметь одинаковые значения в id\[]. Быстрый поиск называется так, потому что для find() требуется только 1 обращение к массиву.

Для объдинения двух компонентов нужно сделать так, чтобы все элементы id\[], соответствующие обоим множествам узлов, имели одно и то же значение. Для этого делается перебор всего массива.

public int find(int p) {

&#x20;   return id\[p];

}

public void union(int p, int q) {

&#x20;   int pID = id\[p];  // needed for correctness

&#x20;   int qID = id\[q];  // to reduce the number of array accesses

&#x20;   // p and q are already in the same component

&#x20;   if (pID == qID) return;

&#x20;   for (int i = 0; i < id.length; i++)

&#x20;       if (id\[i] == pID) id\[i] = qID;

&#x20;   count--;

}

Квадратичное время union() для типичных случаев, когда, в конце концов, остается небольшое количество компонентов.

**Быстрое объединение**



Элемент массива для каждого узла содержит имя другого узла в том же компоненте (или себя) - это связь.

В методе find() мы начинаем с указанного узла, переходим по ссыдке к другому узлу и тд, пока не дойдем до корня(узла указывающего на себя).

Два узла принадлежат одному компоненту, если они приводят к одному корню.

Метод union(), переходя по ссылкам находит корни узлов, а затем, переименовав всего один компонент, привязывает один из корней к другому.

Переименовываемый компонент может быть выбран произвольно.

public int find(int p) {

&#x20;   while (p != parent\[p])

&#x20;       p = parent\[p];

&#x20;   return p;

}

public void union(int p, int q) {

&#x20;   int rootP = find(p);

&#x20;   int rootQ = find(q);

&#x20;   if (rootP == rootQ) return;

&#x20;   id\[rootP] = rootQ;

&#x20;

&#x20;   count--;

}

Квадратичное время find() в худшем случае - когда n узлов принадлежат одному множеству, а дерево имеет высоту n-1

**Взвешенное быстрое объединение**



Вместо произвольного присоединения деревьев, мы будем отслеживать размер каждого дерева и всегда присоединять меньшее дерево к большему.

Потребуется доп массив для хранения счетчиков узлов.

public class WeightedQuickUnionUF {

&#x20;   private int\[] id;        // parent of i

&#x20;   private int\[] size;      // size\[i] = number of sites in subtree rooted at i

&#x20;   private int count;       // number of components

&#x20;   public WeightedQuickUnionUF(int n) {

&#x20;       count = n;

&#x20;       id = new int\[n];

&#x20;       size = new int\[n];

&#x20;       for (int i = 0; i < n; i++) {

&#x20;           id\[i] = i;

&#x20;           size\[i] = 1;

&#x20;   }

&#x20;   public int count() {

&#x20;       return count;

&#x20;   }

&#x20;   public boolean connected(int p, int q) {

&#x20;       return find(p) == find(q);

&#x20;   }

&#x20;   public int find(int p) {       &#x20;

&#x20;       while (p != id\[p])

&#x20;           p = id\[p];

&#x20;       return p;

&#x20;   }

&#x20;   public void union(int p, int q) {

&#x20;       int rootP = find(p);

&#x20;       int rootQ = find(q);

&#x20;       if (rootP == rootQ) return;

&#x20;       // make smaller root point to larger one

&#x20;       if (size\[rootP] < size\[rootQ]) {

&#x20;           id\[rootP] = rootQ;

&#x20;           size\[rootQ] += size\[rootP];

&#x20;       }

&#x20;       else {

&#x20;           id\[rootQ] = rootP;

&#x20;           size\[rootP] += size\[rootQ];

&#x20;       }

&#x20;       count--;

&#x20;   }

}

**Добавление сжатия пути**

В идеале каждый узел был связан непосредственно с корнем его дерева. Приблизиться к идеалу можно, привязывая к корню все узлы, которые мы просматриваем.

Для реализации добавим в метод find() еще один цикл, который заносит в элементы id\[], соответствующие всем узлам на пути к корню, ссылку непосредственно на корень.
