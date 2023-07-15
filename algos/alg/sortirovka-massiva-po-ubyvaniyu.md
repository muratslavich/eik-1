# Сортировка массива по убыванию

public static void sort(int\[] array) {

&#x20;   //напишите тут ваш код

&#x20;   Arrays.sort(array);

&#x20;   int i = 0,j = array.length - 1,tmp;

&#x20;   while (j > i) {

&#x20;       tmp = array\[j];

&#x20;       array\[j] = array\[i];

&#x20;       array\[i] = tmp;

&#x20;       j--;

&#x20;       i++;

&#x20;   }

}
