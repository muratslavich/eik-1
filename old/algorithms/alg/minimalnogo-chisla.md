# минимального числа

Допустим в массиве требуется найти максимальное и минимальное число. Конечно, для этого можно было написать два метода, но при вызове обоих методов массив просматривается дважды. Эффективнее определить в нем максимальное и минимальное значение. Но в этом случае метод должен возвращать два значения. Сделать это можно определив класс Pair с двумя полями для хранения числовых значений, тогда метод minmax() сможет возвратить объект типа Pair.

ТО для получения максимального и минимального числа достаточно вызвать методы getFirst() и getSecond().

Подлинным именем внутреннего класса будет ArrayAlg.Pair p = ArrayAlg.minmax(d);

package staticInnerClass;

/\*\*

&#x20;\* This program demonstrates the use of static inner classes.

&#x20;\* @version 1.01 2004-02-27

&#x20;\* @author Cay Horstmann

&#x20;\*/

public class StaticInnerClassTest

{

&#x20;   public static void main(String\[] args)

&#x20;   {

&#x20;       double\[] d = new double\[20];

&#x20;       for (int i = 0; i < d.length; i++)

&#x20;           d\[i] = 100 \* Math.random();

&#x20;       ArrayAlg.Pair p = ArrayAlg.minmax(d);

&#x20;       System.out.println("min = " + p.getFirst());

&#x20;       System.out.println("max = " + p.getSecond());

&#x20;   }

}

class ArrayAlg

{

&#x20;   /\*\*

&#x20;    \* A pair of floating-point numbers

&#x20;    \*/

&#x20;   **public static class Pair**

&#x20;   {

&#x20;       private double first;

&#x20;       private double second;

&#x20;       /\*\*

&#x20;        \* Constructs a pair from two floating-point numbers

&#x20;        \* @param f the first number

&#x20;        \* @param s the second number

&#x20;        \*/

&#x20;       public Pair(double f, double s)

&#x20;       {

&#x20;           first = f;

&#x20;           second = s;

&#x20;       }

&#x20;       /\*\*

&#x20;        \* Returns the first number of the pair

&#x20;        \* @return the first number

&#x20;        \*/

&#x20;       public double getFirst()

&#x20;       {

&#x20;           return first;

&#x20;       }

&#x20;       /\*\*

&#x20;        \* Returns the second number of the pair

&#x20;        \* @return the second number

&#x20;        \*/

&#x20;       public double getSecond()

&#x20;       {

&#x20;           return second;

&#x20;       }

&#x20;   }

&#x20;   /\*\*

&#x20;    \* Computes both the minimum and the maximum of an array

&#x20;    \* @param values an array of floating-point numbers

&#x20;    \* @return a pair whose first element is the minimum and whose second element

&#x20;    \* is the maximum

&#x20;    \*/

&#x20;   public static Pair minmax(double\[] values)

&#x20;   {

&#x20;       double min = Double.MAX\_VALUE;

&#x20;       double max = Double.MIN\_VALUE;

&#x20;       for (double v : values)

&#x20;       {

&#x20;           if (min > v) min = v;

&#x20;           if (max < v) max = v;

&#x20;       }

&#x20;       return new Pair(min, max);

&#x20;   }

}
