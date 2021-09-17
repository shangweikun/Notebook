# Scala 字符串



以上实例定义了变量 greeting，为字符串常量，它的类型为 **String (java.lang.String)**。

在 Scala 中，字符串的类型实际上是 Java String，它本身没有 String 类。

在 Scala 中，String 是一个不可变的对象，所以该对象不可被修改。这就意味着你如果修改字符串就会产生一个新的字符串对象。



```scala
object Test {
   val greeting: String = "Hello,World!"

   def main(args: Array[String]) {
      println( greeting )
   }
}
```



### create String

```scala
var greeting = "Hello World!";

//或

var greeting:String = "Hello World!";
```



###String's Length

```scala
object Test {
   def main(args: Array[String]) {
      var palindrome = "www.runoob.com";
      var len = palindrome.length();
      println( "String Length is : " + len );
   }
}
```



###Connact String

```scala
"菜鸟教程官网： ".concat("www.runoob.com");
```



###Create formatted String

```scala
object Test {
   def main(args: Array[String]) {
      var floatVar = 12.456
      var intVar = 2000
      var stringVar = "菜鸟教程!"
      var fs = printf("浮点型变量为 " +
                   "%f, 整型变量为 %d, 字符串为 " +
                   " %s", floatVar, intVar, stringVar)
      println(fs)
   }
}
```



## String 方法

下表列出了 java.lang.String 中常用的方法，你可以在 Scala 中使用：

| 序号 | 方法及描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | **char charAt(int index)**返回指定位置的字符                 |
| 2    | **int compareTo(Object o)**比较字符串与对象                  |
| 3    | **int compareTo(String anotherString)**按字典顺序比较两个字符串 |
| 4    | **int compareToIgnoreCase(String str)**按字典顺序比较两个字符串，不考虑大小写 |
| 5    | **String concat(String str)**将指定字符串连接到此字符串的结尾 |
| 6    | **boolean contentEquals(StringBuffer sb)**将此字符串与指定的 StringBuffer 比较。 |
| 7    | **static String copyValueOf(char[] data)**返回指定数组中表示该字符序列的 String |
| 8    | **static String copyValueOf(char[] data, int offset, int count)**返回指定数组中表示该字符序列的 String |
| 9    | **boolean endsWith(String suffix)**测试此字符串是否以指定的后缀结束 |
| 10   | **boolean equals(Object anObject)**将此字符串与指定的对象比较 |
| 11   | **boolean equalsIgnoreCase(String anotherString)**将此 String 与另一个 String 比较，不考虑大小写 |
| 12   | **byte getBytes()**使用平台的默认字符集将此 String 编码为 byte 序列，并将结果存储到一个新的 byte 数组中 |
| 13   | **byte[] getBytes(String charsetName**使用指定的字符集将此 String 编码为 byte 序列，并将结果存储到一个新的 byte 数组中 |
| 14   | **void getChars(int srcBegin, int srcEnd, char[] dst, int dstBegin)**将字符从此字符串复制到目标字符数组 |
| 15   | **int hashCode()**返回此字符串的哈希码                       |
| 16   | **int indexOf(int ch)**返回指定字符在此字符串中第一次出现处的索引 |
| 17   | **int indexOf(int ch, int fromIndex)**返回在此字符串中第一次出现指定字符处的索引，从指定的索引开始搜索 |
| 18   | **int indexOf(String str)**返回指定子字符串在此字符串中第一次出现处的索引 |
| 19   | **int indexOf(String str, int fromIndex)**返回指定子字符串在此字符串中第一次出现处的索引，从指定的索引开始 |
| 20   | **String intern()**返回字符串对象的规范化表示形式            |
| 21   | **int lastIndexOf(int ch)**返回指定字符在此字符串中最后一次出现处的索引 |
| 22   | **int lastIndexOf(int ch, int fromIndex)**返回指定字符在此字符串中最后一次出现处的索引，从指定的索引处开始进行反向搜索 |
| 23   | **int lastIndexOf(String str)**返回指定子字符串在此字符串中最右边出现处的索引 |
| 24   | **int lastIndexOf(String str, int fromIndex)**返回指定子字符串在此字符串中最后一次出现处的索引，从指定的索引开始反向搜索 |
| 25   | **int length()**返回此字符串的长度                           |
| 26   | **boolean matches(String regex)**告知此字符串是否匹配给定的正则表达式 |
| 27   | **boolean regionMatches(boolean ignoreCase, int toffset, String other, int ooffset, int len)**测试两个字符串区域是否相等 |
| 28   | **boolean regionMatches(int toffset, String other, int ooffset, int len)**测试两个字符串区域是否相等 |
| 29   | **String replace(char oldChar, char newChar)**返回一个新的字符串，它是通过用 newChar 替换此字符串中出现的所有 oldChar 得到的 |
| 30   | **String replaceAll(String regex, String replacement**使用给定的 replacement 替换此字符串所有匹配给定的正则表达式的子字符串 |
| 31   | **String replaceFirst(String regex, String replacement)**使用给定的 replacement 替换此字符串匹配给定的正则表达式的第一个子字符串 |
| 32   | **String[] split(String regex)**根据给定正则表达式的匹配拆分此字符串 |
| 33   | **String[] split(String regex, int limit)**根据匹配给定的正则表达式来拆分此字符串 |
| 34   | **boolean startsWith(String prefix)**测试此字符串是否以指定的前缀开始 |
| 35   | **boolean startsWith(String prefix, int toffset)**测试此字符串从指定索引开始的子字符串是否以指定前缀开始。 |
| 36   | **CharSequence subSequence(int beginIndex, int endIndex)**返回一个新的字符序列，它是此序列的一个子序列 |
| 37   | **String substring(int beginIndex)**返回一个新的字符串，它是此字符串的一个子字符串 |
| 38   | **String substring(int beginIndex, int endIndex)**返回一个新字符串，它是此字符串的一个子字符串 |
| 39   | **char[] toCharArray()**将此字符串转换为一个新的字符数组     |
| 40   | **String toLowerCase()**使用默认语言环境的规则将此 String 中的所有字符都转换为小写 |
| 41   | **String toLowerCase(Locale locale)**使用给定 Locale 的规则将此 String 中的所有字符都转换为小写 |
| 42   | **String toString()**返回此对象本身（它已经是一个字符串！）  |
| 43   | **String toUpperCase()**使用默认语言环境的规则将此 String 中的所有字符都转换为大写 |
| 44   | **String toUpperCase(Locale locale)**使用给定 Locale 的规则将此 String 中的所有字符都转换为大写 |
| 45   | **String trim()**删除指定字符串的首尾空白符                  |
| 46   | **static String valueOf(primitive data type x)**返回指定类型参数的字符串表示形式 |





# Scala 数组

Scala 语言中提供的数组是用来存储固定大小的同类型元素。



###Declare Array

```scala
var z:Array[String] = new Array[String](3)

//或

var z = new Array[String](3)
```

```scala
z(0) = "Runoob"; z(1) = "Baidu"; z(4/2) = "Google"
```

```scala
var z = Array("Runoob", "Baidu", "Google")
```

猜测：应该也是连续的存储空间



###Handle Array

using `for`

```scala
object Test {
   def main(args: Array[String]) {
      var myList = Array(1.9, 2.9, 3.4, 3.5)
     
      // 输出所有数组元素
      for ( x <- myList ) {
         println( x )
      }

      // 计算数组所有元素的总和
      var total = 0.0;
      for ( i <- 0 to (myList.length - 1)) {
         total += myList(i);
      }
      println("总和为 " + total);

      // 查找数组中的最大元素
      var max = myList(0);
      for ( i <- 1 to (myList.length - 1) ) {
         if (myList(i) > max) max = myList(i);
      }
      println("最大值为 " + max);
   
   }
}
```



###Multidimensional Arrays

```scala
val myMatrix = Array.ofDim[Int](3, 3)
```



实际用例输出：

```scala
import Array._

object Test {
   def main(args: Array[String]) {
      val myMatrix = Array.ofDim[Int](3, 3)
     
      // 创建矩阵
      for (i <- 0 to 2) {
         for ( j <- 0 to 2) {
            myMatrix(i)(j) = j;
         }
      }
     
      // 打印二维阵列
      for (i <- 0 to 2) {
         for ( j <- 0 to 2) {
            print(" " + myMatrix(i)(j));
         }
         println();
      }
   
   }
}
```



###Merge Arrays

using `concat()`

```scala
import Array._

object Test {
   def main(args: Array[String]) {
      var myList1 = Array(1.9, 2.9, 3.4, 3.5)
      var myList2 = Array(8.9, 7.9, 0.4, 1.5)

      var myList3 =  concat( myList1, myList2)
     
      // 输出所有数组元素
      for ( x <- myList3 ) {
         println( x )
      }
   }
}
```



###Create Interval Array

using `range()`

```scala
import Array._

object Test {
   def main(args: Array[String]) {
      var myList1 = range(10, 20, 2)
      var myList2 = range(10,20)

      // 输出所有数组元素
      for ( x <- myList1 ) {
         print( " " + x )
      }
      println()
      for ( x <- myList2 ) {
         print( " " + x )
      }
   }
}
/*
 * result 
 * 10 12 14 16 18
 * 10 11 12 13 14 15 16 17 18 19
 */
```



## Scala 数组方法

下表中为 Scala 语言中处理数组的重要方法，使用它前我们需要使用 **import Array._** 引入包。

| 序号 | 方法和描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | **def apply( x: T, xs: T\* ): Array[T]**创建指定对象 T 的数组, T 的值可以是 Unit, Double, Float, Long, Int, Char, Short, Byte, Boolean。 |
| 2    | **def concat[T]\( xss: Array[T]\*): Array[T]**合并数组       |
| 3    | **def copy( src: AnyRef, srcPos: Int, dest: AnyRef, destPos: Int, length: Int ): Unit**复制一个数组到另一个数组上。相等于 Java's System.arraycopy(src, srcPos, dest, destPos, length)。 |
| 4    | **def empty[T]: Array[T]**返回长度为 0 的数组                |
| 5    | **def iterate[T]\( start: T, len: Int )( f: (T) => T ): Array[T]**返回指定长度数组，每个数组元素为指定函数的返回值。以上实例数组初始值为 0，长度为 3，计算函数为**a=>a+1**： `scala> Array.iterate(0,3)(a=>a+1)                                           res1: Array[Int] = Array(0, 1, 2)                                                   //` |
| 6    | **def fill[T]\( n: Int )(elem: => T): Array[T]**返回数组，长度为第一个参数指定，同时每个元素使用第二个参数进行填充。 |
| 7    | **def fill[T]\( n1: Int, n2: Int )( elem: => T ): Array[Array[T]]**返回二数组，长度为第一个参数指定，同时每个元素使用第二个参数进行填充。 |
| 8    | **def ofDim[T]\( n1: Int ): Array[T]**创建指定长度的数组     |
| 9    | **def ofDim[T]\( n1: Int, n2: Int ): Array[Array[T]]**创建二维数组 |
| 10   | **def ofDim[T]\( n1: Int, n2: Int, n3: Int ): Array[Array[Array[T]]]**创建三维数组 |
| 11   | **def range( start: Int, end: Int, step: Int ): Array[Int]**创建指定区间内的数组，step 为每个元素间的步长 |
| 12   | **def range( start: Int, end: Int ): Array[Int]**创建指定区间内的数组 |
| 13   | **def tabulate[T]\( n: Int )(f: (Int)=> T): Array[T]**返回指定长度数组，每个数组元素为指定函数的返回值，默认从 0 开始。以上实例返回 3 个元素：`scala> Array.tabulate(3)(a => a + 5) res0: Array[Int] = Array(5, 6, 7)` |
| 14   | **def tabulate[T]\( n1: Int, n2: Int )( f: (Int, Int ) => T): Array[Array[T]]**返回指定长度的二维数组，每个数组元素为指定函数的返回值，默认从 0 开始。 |





# FOR example

First:

```scala
object Test {
   def main(args: Array[String]) {
      var a = 0;
      // for 循环
      for( a <- 1 to 10){
         println( "Value of a: " + a );
      }
   }
}
```

Second:

```scala
object Test {
   def main(args: Array[String]) {
      var a = 0;
      // for 循环
      for( a <- 1 until 10){
         println( "Value of a: " + a );
      }
   }
```

Third:

```scala
object Test {
   def main(args: Array[String]) {
      var a = 0;
      var b = 0;
      // for 循环
      for( a <- 1 to 3; b <- 1 to 3){
         println( "Value of a: " + a );
         println( "Value of b: " + b );
      }
   }
}
```

Fourth:

```scala
object Test {
   def main(args: Array[String]) {
      var a = 0;
      val numList = List(1,2,3,4,5,6);

      // for 循环
      for( a <- numList ){
         println( "Value of a: " + a );
      }
   }
}
```

Firth:

```scala
object Test {
   def main(args: Array[String]) {
      var a = 0;
      val numList = List(1,2,3,4,5,6,7,8,9,10);

      // for 循环
      for( a <- numList
           if a != 3; if a < 8 ){
         println( "Value of a: " + a );
      }
   }
}
```

Sixth:

```scala
object Test {
   def main(args: Array[String]) {
      var a = 0;
      val numList = List(1,2,3,4,5,6,7,8,9,10);

      // for 循环
      var retVal = for{ a <- numList
                        if a != 3; if a < 8
                      }yield a

      // 输出返回值
      for( a <- retVal){
         println( "Value of a: " + a );
      }
   }
}
```





