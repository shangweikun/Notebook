# Scala List(列表)

Scala 列表类似于数组，它们所有元素的类型都相同，但是它们也有所不同：列表是不可变的，值一旦被定义了就不能改变，其次列表 具有递归的结构（也就是链接表结构）而数组不是。。

```scala
// 字符串列表
val site: List[String] = List("Runoob", "Google", "Baidu")

// 整型列表
val nums: List[Int] = List(1, 2, 3, 4)

// 空列表
val empty: List[Nothing] = List()

// 二维列表
val dim: List[List[Int]] =
   List(
      List(1, 0, 0),
      List(0, 1, 0),
      List(0, 0, 1)
   )
```



构造列表的两个基本单位是 **Nil** 和 **::**

**Nil** 也可以表示为一个空列表。

以上实例我们可以写成如下所示：

```scala
// 字符串列表
val site = "Runoob" :: ("Google" :: ("Baidu" :: Nil))

// 整型列表
val nums = 1 :: (2 :: (3 :: (4 :: Nil)))

// 空列表
val empty = Nil

// 二维列表
val dim = (1 :: (0 :: (0 :: Nil))) ::
          (0 :: (1 :: (0 :: Nil))) ::
          (0 :: (0 :: (1 :: Nil))) :: Nil



## 列表基本操作

Scala列表有三个基本操作：

- `head` 返回列表第一个元素
- `tail` 返回一个列表，包含除了第一元素之外的其他元素
- `isEmpty` 在列表为空时返回true

​```scala
// 字符串列表
object Test {
   def main(args: Array[String]) {
      val site = "Runoob" :: ("Google" :: ("Baidu" :: Nil))
      val nums = Nil

      println( "第一网站是 : " + site.head )
      println( "最后一个网站是 : " + site.tail )
      println( "查看列表 site 是否为空 : " + site.isEmpty )
      println( "查看 nums 是否为空 : " + nums.isEmpty )
   }
}
```



## 连接列表

你可以使用 **:::** 运算符或 **List.:::()** 方法或 **List.concat()** 方法来连接两个或多个列表。实例如下

```scala
object Test {
   def main(args: Array[String]) {
      val site1 = "Runoob" :: ("Google" :: ("Baidu" :: Nil))
      val site2 = "Facebook" :: ("Taobao" :: Nil)

      // 使用 ::: 运算符
      var fruit = site1 ::: site2
      println( "site1 ::: site2 : " + fruit )
     
      // 使用 List.:::() 方法
      fruit = site1.:::(site2)
      println( "site1.:::(site2) : " + fruit )

      // 使用 concat 方法
      fruit = List.concat(site1, site2)
      println( "List.concat(site1, site2) : " + fruit  )
     
   }
}
/*
 * site1 ::: site2 : List(Runoob, Google, Baidu, Facebook, Taobao)
 * site1.:::(site2) : List(Facebook, Taobao, Runoob, Google, Baidu)
 * List.concat(site1, site2) : List(Runoob, Google, Baidu, Facebook, Taobao)
 */
```



## List.fill()

我们可以使用 List.fill() 方法来创建一个指定重复数量的元素列表：

```scala
object Test {
   def main(args: Array[String]) {
      val site = List.fill(3)("Runoob") // 重复 Runoob 3次
      println( "site : " + site  )

      val num = List.fill(10)(2)         // 重复元素 2, 10 次
      println( "num : " + num  )
   }
}
```



## List.tabulate()

List.tabulate() 方法是通过给定的函数来创建列表。

方法的第一个参数为元素的数量，可以是二维的，第二个参数为指定的函数，我们通过指定的函数计算结果并返回值插入到列表中，起始值为 0，实例如下

```scala
object Test {
   def main(args: Array[String]) {
      // 通过给定的函数创建 5 个元素
      val squares = List.tabulate(6)(n => n * n)
      println( "一维 : " + squares  )

      // 创建二维列表
      val mul = List.tabulate( 4,5 )( _ * _ )      
      println( "多维 : " + mul  )
   }
}
```



## List.reverse

List.reverse 用于将列表的顺序反转，实例如下：

```scala
object Test {
   def main(args: Array[String]) {
      val site = "Runoob" :: ("Google" :: ("Baidu" :: Nil))
      println( "site 反转前 : " + site )

      println( "site 反转后 : " + site.reverse )
   }
}
```



## Scala List 常用方法

下表列出了 Scala List 常用的方法：

| 序号 | 方法及描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | **def +:(elem: A): List[A]**为列表预添加元素                 |
| 2    | **def ::(x: A): List[A]**在列表开头添加元素                  |
| 3    | **def :::(prefix: List[A]): List[A]**在列表开头添加指定列表的元素 |
| 4    | **def :+(elem: A): List[A]**复制添加元素后列表。             |
| 5    | **def addString(b: StringBuilder): StringBuilder**将列表的所有元素添加到 StringBuilder |
| 6    | **def addString(b: StringBuilder, sep: String): StringBuilder**将列表的所有元素添加到 StringBuilder，并指定分隔符 |
| 7    | **def apply(n: Int): A**通过列表索引获取元素                 |
| 8    | **def contains(elem: Any): Boolean**检测列表中是否包含指定的元素 |
| 9    | **def copyToArray(xs: Array[A], start: Int, len: Int): Unit**将列表的元素复制到数组中。 |
| 10   | **def distinct: List[A]**去除列表的重复元素，并返回新列表    |
| 11   | **def drop(n: Int): List[A]**丢弃前n个元素，并返回新列表     |
| 12   | **def dropRight(n: Int): List[A]**丢弃最后n个元素，并返回新列表 |
| 13   | **def dropWhile(p: (A) => Boolean): List[A]**从左向右丢弃元素，直到条件p不成立 |
| 14   | **def endsWith[B]\(that: Seq[B]): Boolean**检测列表是否以指定序列结尾 |
| 15   | **def equals(that: Any): Boolean**判断是否相等               |
| 16   | **def exists(p: (A) => Boolean): Boolean**判断列表中指定条件的元素是否存在。判断l是否存在某个元素:`scala> l.exists(s => s == "Hah") res7: Boolean = true` |
| 17   | **def filter(p: (A) => Boolean): List[A]**输出符号指定条件的所有元素。过滤出长度为3的元素:`scala> l.filter(s => s.length == 3) res8: List[String] = List(Hah, WOW)` |
| 18   | **def forall(p: (A) => Boolean): Boolean**检测所有元素。例如：判断所有元素是否以"H"开头：scala> l.forall(s => s.startsWith("H")) res10: Boolean = false |
| 19   | **def foreach(f: (A) => Unit): Unit**将函数应用到列表的所有元素 |
| 20   | **def head: A**获取列表的第一个元素                          |
| 21   | **def indexOf(elem: A, from: Int): Int**从指定位置 from 开始查找元素第一次出现的位置 |
| 22   | **def init: List[A]**返回所有元素，除了最后一个              |
| 23   | **def intersect(that: Seq[A]): List[A]**计算多个集合的交集   |
| 24   | **def isEmpty: Boolean**检测列表是否为空                     |
| 25   | **def iterator: Iterator[A]**创建一个新的迭代器来迭代元素    |
| 26   | **def last: A**返回最后一个元素                              |
| 27   | **def lastIndexOf(elem: A, end: Int): Int**在指定的位置 end 开始查找元素最后出现的位置 |
| 28   | **def length: Int**返回列表长度                              |
| 29   | **def map[B]\(f: (A) => B): List[B]**通过给定的方法将所有元素重新计算 |
| 30   | **def max: A**查找最大元素                                   |
| 31   | **def min: A**查找最小元素                                   |
| 32   | **def mkString: String**列表所有元素作为字符串显示           |
| 33   | **def mkString(sep: String): String**使用分隔符将列表所有元素作为字符串显示 |
| 34   | **def reverse: List[A]**列表反转                             |
| 35   | **def sorted[B >: A]: List[A]**列表排序                      |
| 36   | **def startsWith[B]\(that: Seq[B], offset: Int): Boolean**检测列表在指定位置是否包含指定序列 |
| 37   | **def sum: A**计算集合元素之和                               |
| 38   | **def tail: List[A]**返回所有元素，除了第一个                |
| 39   | **def take(n: Int): List[A]**提取列表的前n个元素             |
| 40   | **def takeRight(n: Int): List[A]**提取列表的后n个元素        |
| 41   | **def toArray: Array[A]**列表转换为数组                      |
| 42   | **def toBuffer[B >: A]: Buffer[B]**返回缓冲区，包含了列表的所有元素 |
| 43   | **def toMap[T, U]: Map[T, U]**List 转换为 Map                |
| 44   | **def toSeq: Seq[A]**List 转换为 Seq                         |
| 45   | **def toSet[B >: A]: Set[B]**List 转换为 Set                 |
| 46   | **def toString(): String**列表转换为字符串                   |



-------



# Scala Set(集合)

Scala Set(集合)是没有重复的对象集合，所有的元素都是唯一的。

Scala 集合分为可变的和不可变的集合。

默认情况下，Scala 使用的是不可变集合，如果你想使用可变集合，需要引用 **scala.collection.mutable.Set** 包。

默认引用 **scala.collection.immutable.Set**

```scala
import scala.collection.mutable.Set // 可以在任何地方引入 可变集合

val mutableSet = Set(1,2,3)
println(mutableSet.getClass.getName) // scala.collection.mutable.HashSet

mutableSet.add(4)
mutableSet.remove(1)
mutableSet += 5
mutableSet -= 2

println(mutableSet) // Set(5, 3, 4)

val another = mutableSet.toSet
println(another.getClass.getName) // scala.collection.immutable.Set
```



**注意：** *虽然可变Set和不可变Set都有添加或删除元素的操作，但是有一个非常大的差别。对不可变Set进行操作，会产生一个新的set，原来的set并没有改变，这与List一样。 而对可变Set进行操作，改变的是该Set本身，与ListBuffer类似。*



## 集合基本操作

Scala集合有三个基本操作：

- `head` 返回集合第一个元素
- `tail` 返回一个集合，包含除了第一元素之外的其他元素
- `isEmpty` 在集合为空时返回true

对于Scala集合的任何操作都可以使用这三个基本操作来表达。实例如下:

```scala
object Test {
   def main(args: Array[String]) {
      val site = Set("Runoob", "Google", "Baidu")
      val nums: Set[Int] = Set()

      println( "第一网站是 : " + site.head )
      println( "最后一个网站是 : " + site.tail )
      println( "查看列表 site 是否为空 : " + site.isEmpty )
      println( "查看 nums 是否为空 : " + nums.isEmpty )
   }
}
```



## 连接集合

你可以使用 **++** 运算符或 **Set.++()** 方法来连接两个集合。如果元素有重复的就会移除重复的元素。

```scala
object Test {
   def main(args: Array[String]) {
      val site1 = Set("Runoob", "Google", "Baidu")
      val site2 = Set("Faceboook", "Taobao")

      // ++ 作为运算符使用
      var site = site1 ++ site2
      println( "site1 ++ site2 : " + site )

      //  ++ 作为方法使用
      site = site1.++(site2)
      println( "site1.++(site2) : " + site )
   }
}
```



## 查找集合中最大与最小元素

你可以使用 **Set.min** 方法来查找集合中的最小元素，使用 **Set.max** 方法查找集合中的最大元素。

```scala
object Test {
   def main(args: Array[String]) {
      val num = Set(5,6,9,20,30,45)

      // 查找集合中最大与最小元素
      println( "Set(5,6,9,20,30,45) 集合中的最小元素是 : " + num.min )
      println( "Set(5,6,9,20,30,45) 集合中的最大元素是 : " + num.max )
   }
}
```



## 交集

你可以使用 **Set.&** 方法或 **Set.intersect** 方法来查看两个集合的交集元素。

```scala
object Test {
   def main(args: Array[String]) {
      val num1 = Set(5,6,9,20,30,45)
      val num2 = Set(50,60,9,20,35,55)

      // 交集
      println( "num1.&(num2) : " + num1.&(num2) )
      println( "num1.intersect(num2) : " + num1.intersect(num2) )
   }
}
```



## Scala Set 常用方法

下表列出了 Scala Set 常用的方法：

| 序号 | 方法及描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | **def +(elem: A): Set[A]**为集合添加新元素，x并创建一个新的集合，除非元素已存在 |
| 2    | **def -(elem: A): Set[A]**移除集合中的元素，并创建一个新的集合 |
| 3    | **def contains(elem: A): Boolean**如果元素在集合中存在，返回 true，否则返回 false。 |
| 4    | **def &(that: Set[A]): Set[A]**返回两个集合的交集            |
| 5    | **def &~(that: Set[A]): Set[A]**返回两个集合的差集           |
| 6    | **def +(elem1: A, elem2: A, elems: A\*): Set[A]**通过添加传入指定集合的元素创建一个新的不可变集合 |
| 7    | **def ++(elems: A): Set[A]**合并两个集合                     |
| 8    | **def -(elem1: A, elem2: A, elems: A\*): Set[A]**通过移除传入指定集合的元素创建一个新的不可变集合 |
| 9    | **def addString(b: StringBuilder): StringBuilder**将不可变集合的所有元素添加到字符串缓冲区 |
| 10   | **def addString(b: StringBuilder, sep: String): StringBuilder**将不可变集合的所有元素添加到字符串缓冲区，并使用指定的分隔符 |
| 11   | **def apply(elem: A)**检测集合中是否包含指定元素             |
| 12   | **def count(p: (A) => Boolean): Int**计算满足指定条件的集合元素个数 |
| 13   | **def copyToArray(xs: Array[A], start: Int, len: Int): Unit**复制不可变集合元素到数组 |
| 14   | **def diff(that: Set[A]): Set[A]**比较两个集合的差集         |
| 15   | **def drop(n: Int): Set[A]]**返回丢弃前n个元素新集合         |
| 16   | **def dropRight(n: Int): Set[A]**返回丢弃最后n个元素新集合   |
| 17   | **def dropWhile(p: (A) => Boolean): Set[A]**从左向右丢弃元素，直到条件p不成立 |
| 18   | **def equals(that: Any): Boolean**equals 方法可用于任意序列。用于比较系列是否相等。 |
| 19   | **def exists(p: (A) => Boolean): Boolean**判断不可变集合中指定条件的元素是否存在。 |
| 20   | **def filter(p: (A) => Boolean): Set[A]**输出符合指定条件的所有不可变集合元素。 |
| 21   | **def find(p: (A) => Boolean): Option[A]**查找不可变集合中满足指定条件的第一个元素 |
| 22   | **def forall(p: (A) => Boolean): Boolean**查找指定条件是否适用于此集合的所有元素 |
| 23   | **def foreach(f: (A) => Unit): Unit**将函数应用到不可变集合的所有元素 |
| 24   | **def head: A**获取不可变集合的第一个元素                    |
| 25   | **def init: Set[A]**返回所有元素，除了最后一个               |
| 26   | **def intersect(that: Set[A]): Set[A]**计算两个集合的交集    |
| 27   | **def isEmpty: Boolean**判断集合是否为空                     |
| 28   | **def iterator: Iterator[A]**创建一个新的迭代器来迭代元素    |
| 29   | **def last: A**返回最后一个元素                              |
| 30   | **def map[B]\(f: (A) => B): immutable.Set[B]**通过给定的方法将所有元素重新计算 |
| 31   | **def max: A**查找最大元素                                   |
| 32   | **def min: A**查找最小元素                                   |
| 33   | **def mkString: String**集合所有元素作为字符串显示           |
| 34   | **def mkString(sep: String): String**使用分隔符将集合所有元素作为字符串显示 |
| 35   | **def product: A**返回不可变集合中数字元素的积。             |
| 36   | **def size: Int**返回不可变集合元素的数量                    |
| 37   | **def splitAt(n: Int): (Set[A], Set[A])**把不可变集合拆分为两个容器，第一个由前 n 个元素组成，第二个由剩下的元素组成 |
| 38   | **def subsetOf(that: Set[A]): Boolean**如果集合中含有子集返回 true，否则返回false |
| 39   | **def sum: A**返回不可变集合中所有数字元素之和               |
| 40   | **def tail: Set[A]**返回一个不可变集合中除了第一元素之外的其他元素 |
| 41   | **def take(n: Int): Set[A]**返回前 n 个元素                  |
| 42   | **def takeRight(n: Int):Set[A]**返回后 n 个元素              |
| 43   | **def toArray: Array[A]**将集合转换为数组                    |
| 44   | **def toBuffer[B >: A]: Buffer[B]**返回缓冲区，包含了不可变集合的所有元素 |
| 45   | **def toList: List[A]**返回 List，包含了不可变集合的所有元素 |
| 46   | **def toMap[T, U]: Map[T, U]**返回 Map，包含了不可变集合的所有元素 |
| 47   | **def toSeq: Seq[A]**返回 Seq，包含了不可变集合的所有元素    |
| 48   | **def toString(): String**返回一个字符串，以对象来表示       |



-----



# Scala Map(映射)



Map(映射)是一种可迭代的键值对（key/value）结构。

所有的值都可以通过键来获取。

Map 中的键都是唯一的。

Map 也叫哈希表（Hash tables）。

Map 有两种类型，可变与不可变，区别在于可变对象可以修改它，而不可变对象不可以。

默认情况下 Scala 使用不可变 Map。如果你需要使用可变集合，你需要显式的引入 **import scala.collection.mutable.Map** 类

在 Scala 中 你可以同时使用可变与不可变 Map，不可变的直接使用 Map，可变的使用 mutable.Map。

```scala
// 空哈希表，键为字符串，值为整型
var A:Map[Char,Int] = Map()

// Map 键值对演示
val colors = Map("red" -> "#FF0000", "azure" -> "#F0FFFF")
```



## Map 基本操作

Scala Map 有三个基本操作：

| 方法    | 描述                     |
| :------ | :----------------------- |
| keys    | 返回 Map 所有的键(key)   |
| values  | 返回 Map 所有的值(value) |
| isEmpty | 在 Map 为空时返回true    |

```scala
object Test {
   def main(args: Array[String]) {
      val colors = Map("red" -> "#FF0000",
                       "azure" -> "#F0FFFF",
                       "peru" -> "#CD853F")

      val nums: Map[Int, Int] = Map()

      println( "colors 中的键为 : " + colors.keys )
      println( "colors 中的值为 : " + colors.values )
      println( "检测 colors 是否为空 : " + colors.isEmpty )
      println( "检测 nums 是否为空 : " + nums.isEmpty )
   }
}
```



## Map 合并

你可以使用 **++** 运算符或 **Map.++()** 方法来连接两个 Map，Map 合并时会移除重复的 key。

```scala
object Test {
   def main(args: Array[String]) {
      val colors1 = Map("red" -> "#FF0000",
                        "azure" -> "#F0FFFF",
                        "peru" -> "#CD853F")
      val colors2 = Map("blue" -> "#0033FF",
                        "yellow" -> "#FFFF00",
                        "red" -> "#FF0000")

      //  ++ 作为运算符
      var colors = colors1 ++ colors2
      println( "colors1 ++ colors2 : " + colors )

      //  ++ 作为方法
      colors = colors1.++(colors2)
      println( "colors1.++(colors2) : " + colors )

   }
}
```

## 输出 Map 的 keys 和 values

以下通过 foreach 循环输出 Map 中的 keys 和 values：

```scala
object Test {
   def main(args: Array[String]) {
      val sites = Map("runoob" -> "http://www.runoob.com",
                       "baidu" -> "http://www.baidu.com",
                       "taobao" -> "http://www.taobao.com")

      sites.keys.foreach{ i =>  
                           print( "Key = " + i )
                           println(" Value = " + sites(i) )}
   }
}
```



## 查看 Map 中是否存在指定的 Key

你可以使用 **Map.contains** 方法来查看 Map 中是否存在指定的 Key。

```scala
object Test {
   def main(args: Array[String]) {
      val sites = Map("runoob" -> "http://www.runoob.com",
                       "baidu" -> "http://www.baidu.com",
                       "taobao" -> "http://www.taobao.com")

      if( sites.contains( "runoob" )){
           println("runoob 键存在，对应的值为 :"  + sites("runoob"))
      }else{
           println("runoob 键不存在")
      }
      if( sites.contains( "baidu" )){
           println("baidu 键存在，对应的值为 :"  + sites("baidu"))
      }else{
           println("baidu 键不存在")
      }
      if( sites.contains( "google" )){
           println("google 键存在，对应的值为 :"  + sites("google"))
      }else{
           println("google 键不存在")
      }
   }
}
```



## Scala Map 方法

下表列出了 Scala Map 常用的方法：

| 序号 | 方法及描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | **def ++(xs: Map[(A, B)]): Map[A, B]**返回一个新的 Map，新的 Map xs 组成 |
| 2    | **def -(elem1: A, elem2: A, elems: A\*): Map[A, B]**返回一个新的 Map, 移除 key 为 elem1, elem2 或其他 elems。 |
| 3    | **def --(xs: GTO[A]): Map[A, B]**返回一个新的 Map, 移除 xs 对象中对应的 key |
| 4    | **def get(key: A): Option[B]**返回指定 key 的值              |
| 5    | **def iterator: Iterator[(A, B)]**创建新的迭代器，并输出 key/value 对 |
| 6    | **def addString(b: StringBuilder): StringBuilder**将 Map 中的所有元素附加到StringBuilder，可加入分隔符 |
| 7    | **def addString(b: StringBuilder, sep: String): StringBuilder**将 Map 中的所有元素附加到StringBuilder，可加入分隔符 |
| 8    | **def apply(key: A): B**返回指定键的值，如果不存在返回 Map 的默认方法 |
| 9    | **def clear(): Unit**清空 Map                                |
| 10   | **def clone(): Map[A, B]**从一个 Map 复制到另一个 Map        |
| 11   | **def contains(key: A): Boolean**如果 Map 中存在指定 key，返回 true，否则返回 false。 |
| 12   | **def copyToArray(xs: Array[(A, B)]): Unit**复制集合到数组   |
| 13   | **def count(p: ((A, B)) => Boolean): Int**计算满足指定条件的集合元素数量 |
| 14   | **def default(key: A): B**定义 Map 的默认值，在 key 不存在时返回。 |
| 15   | **def drop(n: Int): Map[A, B]**返回丢弃前n个元素新集合       |
| 16   | **def dropRight(n: Int): Map[A, B]**返回丢弃最后n个元素新集合 |
| 17   | **def dropWhile(p: ((A, B)) => Boolean): Map[A, B]**从左向右丢弃元素，直到条件p不成立 |
| 18   | **def empty: Map[A, B]**返回相同类型的空 Map                 |
| 19   | **def equals(that: Any): Boolean**如果两个 Map 相等(key/value 均相等)，返回true，否则返回false |
| 20   | **def exists(p: ((A, B)) => Boolean): Boolean**判断集合中指定条件的元素是否存在 |
| 21   | **def filter(p: ((A, B))=> Boolean): Map[A, B]**返回满足指定条件的所有集合 |
| 22   | **def filterKeys(p: (A) => Boolean): Map[A, B]**返回符合指定条件的不可变 Map |
| 23   | **def find(p: ((A, B)) => Boolean): Option[(A, B)]**查找集合中满足指定条件的第一个元素 |
| 24   | **def foreach(f: ((A, B)) => Unit): Unit**将函数应用到集合的所有元素 |
| 25   | **def init: Map[A, B]**返回所有元素，除了最后一个            |
| 26   | **def isEmpty: Boolean**检测 Map 是否为空                    |
| 27   | **def keys: Iterable[A]**返回所有的key/p>                    |
| 28   | **def last: (A, B)**返回最后一个元素                         |
| 29   | **def max: (A, B)**查找最大元素                              |
| 30   | **def min: (A, B)**查找最小元素                              |
| 31   | **def mkString: String**集合所有元素作为字符串显示           |
| 32   | **def product: (A, B)**返回集合中数字元素的积。              |
| 33   | **def remove(key: A): Option[B]**移除指定 key                |
| 34   | **def retain(p: (A, B) => Boolean): Map.this.type**如果符合满足条件的返回 true |
| 35   | **def size: Int**返回 Map 元素的个数                         |
| 36   | **def sum: (A, B)**返回集合中所有数字元素之和                |
| 37   | **def tail: Map[A, B]**返回一个集合中除了第一元素之外的其他元素 |
| 38   | **def take(n: Int): Map[A, B]**返回前 n 个元素               |
| 39   | **def takeRight(n: Int): Map[A, B]**返回后 n 个元素          |
| 40   | **def takeWhile(p: ((A, B)) => Boolean): Map[A, B]**返回满足指定条件的元素 |
| 41   | **def toArray: Array[(A, B)]**集合转数组                     |
| 42   | **def toBuffer[B >: A]: Buffer[B]**返回缓冲区，包含了 Map 的所有元素 |
| 43   | **def toList: List[A]**返回 List，包含了 Map 的所有元素      |
| 44   | **def toSeq: Seq[A]**返回 Seq，包含了 Map 的所有元素         |
| 45   | **def toSet: Set[A]**返回 Set，包含了 Map 的所有元素         |
| 46   | **def toString(): String**返回字符串对象                     |



# Scala 元组

与列表一样，元组也是不可变的，但与列表不同的是元组可以包含不同类型的元素。

元组的值是通过将单个的值包含在圆括号中构成的。

```scala
val t = (1, 3.14, "Fred")  

val t = new Tuple3(1, 3.14, "Fred")
```



```scala
object Test {
   def main(args: Array[String]) {
      val t = (4,3,2,1)

      val sum = t._1 + t._2 + t._3 + t._4

      println( "元素之和为: "  + sum )
   }
}
```



## 迭代元组

你可以使用 **Tuple.productIterator()** 方法来迭代输出元组的所有元素

```scala
object Test {
   def main(args: Array[String]) {
      val t = (4,3,2,1)
     
      t.productIterator.foreach{ i =>println("Value = " + i )}
   }
}
```



## 元组转为字符串

你可以使用 **Tuple.toString()** 方法将元组的所有元素组合成一个字符串

```scala
object Test {
   def main(args: Array[String]) {
      val t = new Tuple3(1, "hello", Console)
     
      println("连接后的字符串为: " + t.toString() )
   }
}
```

```shell
$ scalac Test.scala 
$ scala Test
连接后的字符串为: (1,hello,scala.Console$@4dd8dc3)
```



## 元素交换

你可以使用 **Tuple.swap** 方法来交换元组的元素。

```scala
object Test {
   def main(args: Array[String]) {
      val t = new Tuple2("www.google.com", "www.runoob.com")
     
      println("交换后的元组: " + t.swap )
   }
}
```



# Scala Option(选项)

Scala Option(选项)类型用来表示一个值是可选的（有值或无值)。

Option[T] 是一个类型为 T 的可选值的容器： 如果值存在， Option[T] 就是一个 Some[T] ，如果不存在， Option[T] 就是对象 None 。

```scala
// 虽然 Scala 可以不定义变量的类型，不过为了清楚些，我还是
// 把他显示的定义上了
 
val myMap: Map[String, String] = Map("key1" -> "value")
val value1: Option[String] = myMap.get("key1")
val value2: Option[String] = myMap.get("key2")
 
println(value1) // Some("value1")
println(value2) // None
```



Option 有两个子类别，一个是 Some，一个是 None，当他回传 Some 的时候，代表这个函式成功地给了你一个 String，而你可以透过 get() 这个函式拿到那个 String，如果他返回的是 None，则代表没有字符串可以给你。

```scala
object Test {
   def main(args: Array[String]) {
      val sites = Map("runoob" -> "www.runoob.com", "google" -> "www.google.com")
     
      println("sites.get( \"runoob\" ) : " +  sites.get( "runoob" )) // Some(www.runoob.com)
      println("sites.get( \"baidu\" ) : " +  sites.get( "baidu" ))  //  None
   }
}
```

```shell
$ scalac Test.scala 
$ scala Test
sites.get( "runoob" ) : Some(www.runoob.com)
sites.get( "baidu" ) : None
```



你也可以通过模式匹配来输出匹配值

```scala
object Test {
   def main(args: Array[String]) {
      val sites = Map("runoob" -> "www.runoob.com", "google" -> "www.google.com")
     
      println("show(sites.get( \"runoob\")) : " +  
                                          show(sites.get( "runoob")) )
      println("show(sites.get( \"baidu\")) : " +  
                                          show(sites.get( "baidu")) )
   }
   
   def show(x: Option[String]) = x match {
      case Some(s) => s
      case None => "?"
   }
}
```



## getOrElse() 方法

你可以使用 getOrElse() 方法来获取元组中存在的元素或者使用其默认的值。

```scala
object Test {
   def main(args: Array[String]) {
      val a:Option[Int] = Some(5)
      val b:Option[Int] = None
     
      println("a.getOrElse(0): " + a.getOrElse(0) )
      println("b.getOrElse(10): " + b.getOrElse(10) )
   }
}
```



```shell
$ scalac Test.scala 
$ scala Test
a.getOrElse(0): 5
b.getOrElse(10): 10
```



## isEmpty() 方法

你可以使用 isEmpty() 方法来检测元组中的元素是否为 None

```scala
object Test {
   def main(args: Array[String]) {
      val a:Option[Int] = Some(5)
      val b:Option[Int] = None
     
      println("a.isEmpty: " + a.isEmpty )
      println("b.isEmpty: " + b.isEmpty )
   }
}
```



## Scala Option 常用方法

下表列出了 Scala Option 常用的方法：

| 序号 | 方法及描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | **def get: A**获取可选值                                     |
| 2    | **def isEmpty: Boolean**检测可选类型值是否为 None，是的话返回 true，否则返回 false |
| 3    | **def productArity: Int**返回元素个数， A(x_1, ..., x_k), 返回 k |
| 4    | **def productElement(n: Int): Any**获取指定的可选项，以 0 为起始。即 A(x_1, ..., x_k), 返回 x_(n+1) ， 0 < n < k. |
| 5    | **def exists(p: (A) => Boolean): Boolean**如果可选项中指定条件的元素存在且不为 None 返回 true，否则返回 false。 |
| 6    | **def filter(p: (A) => Boolean): Option[A]**如果选项包含有值，而且传递给 filter 的条件函数返回 true， filter 会返回 Some 实例。 否则，返回值为 None 。 |
| 7    | **def filterNot(p: (A) => Boolean): Option[A]**如果选项包含有值，而且传递给 filter 的条件函数返回 false， filter 会返回 Some 实例。 否则，返回值为 None 。 |
| 8    | **def flatMap[B]\(f: (A) => Option[B]): Option[B]**如果选项包含有值，则传递给函数 f 处理后返回，否则返回 None |
| 9    | **def foreach[U]\(f: (A) => U): Unit**如果选项包含有值，则将每个值传递给函数 f， 否则不处理。 |
| 10   | **def getOrElse[B >: A]\(default: => B): B**如果选项包含有值，返回选项值，否则返回设定的默认值。 |
| 11   | **def isDefined: Boolean**如果可选值是 Some 的实例返回 true，否则返回 false。 |
| 12   | **def iterator: Iterator[A]**如果选项包含有值，迭代出可选值。如果可选值为空则返回空迭代器。 |
| 13   | **def map[B]\(f: (A) => B): Option[B]**如果选项包含有值， 返回由函数 f 处理后的 Some，否则返回 None |
| 14   | **def orElse[B >: A]\(alternative: => Option[B]): Option[B]**如果一个 Option 是 None ， orElse 方法会返回传名参数的值，否则，就直接返回这个 Option。 |
| 15   | **def orNull**如果选项包含有值返回选项值，否则返回 null。    |



# Scala Iterator（迭代器）

Scala Iterator（迭代器）不是一个集合，它是一种用于访问集合的方法。

迭代器 it 的两个基本操作是 **next** 和 **hasNext**。

调用 **it.next()** 会返回迭代器的下一个元素，并且更新迭代器的状态。

调用 **it.hasNext()** 用于检测集合中是否还有元素。

让迭代器 it 逐个返回所有元素最简单的方法是使用 while 循环：

```scala
object Test {
   def main(args: Array[String]) {
      val it = Iterator("Baidu", "Google", "Runoob", "Taobao")
     
      while (it.hasNext){
         println(it.next())
      }
   }
}
```



## 查找最大与最小元素

你可以使用 **it.min** 和 **it.max** 方法从迭代器中查找最大与最小元素，实例如下:

```scala
object Test {
   def main(args: Array[String]) {
      val ita = Iterator(20,40,2,50,69, 90)
      val itb = Iterator(20,40,2,50,69, 90)
     
      println("最大元素是：" + ita.max )
      println("最小元素是：" + itb.min )

   }
}
```



## 获取迭代器的长度

你可以使用 **it.size** 或 **it.length** 方法来查看迭代器中的元素个数。

```scala
object Test {
   def main(args: Array[String]) {
      val ita = Iterator(20,40,2,50,69, 90)
      val itb = Iterator(20,40,2,50,69, 90)
     
      println("ita.size 的值: " + ita.size )
      println("itb.length 的值: " + itb.length )

   }
}
```



## Scala Iterator 常用方法

下表列出了 Scala Iterator 常用的方法：

| 序号 | 方法及描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | **def hasNext: Boolean**如果还有可返回的元素，返回true。     |
| 2    | **def next(): A**返回迭代器的下一个元素，并且更新迭代器的状态 |
| 3    | **def ++(that: => Iterator[A]): Iterator[A]**合并两个迭代器  |
| 4    | **def ++[B >: A]\(that :=> GenTraversableOnce[B]): Iterator[B]**合并两个迭代器 |
| 5    | **def addString(b: StringBuilder): StringBuilder**添加一个字符串到 StringBuilder b |
| 6    | **def addString(b: StringBuilder, sep: String): StringBuilder**添加一个字符串到 StringBuilder b，并指定分隔符 |
| 7    | **def buffered: BufferedIterator[A]**迭代器都转换成 BufferedIterator |
| 8    | **def contains(elem: Any): Boolean**检测迭代器中是否包含指定元素 |
| 9    | **def copyToArray(xs: Array[A], start: Int, len: Int): Unit**将迭代器中选定的值传给数组 |
| 10   | **def count(p: (A) => Boolean): Int**返回迭代器元素中满足条件p的元素总数。 |
| 11   | **def drop(n: Int): Iterator[A]**返回丢弃前n个元素新集合     |
| 12   | **def dropWhile(p: (A) => Boolean): Iterator[A]**从左向右丢弃元素，直到条件p不成立 |
| 13   | **def duplicate: (Iterator[A], Iterator[A])**生成两个能分别返回迭代器所有元素的迭代器。 |
| 14   | **def exists(p: (A) => Boolean): Boolean**返回一个布尔值，指明迭代器元素中是否存在满足p的元素。 |
| 15   | **def filter(p: (A) => Boolean): Iterator[A]**返回一个新迭代器 ，指向迭代器元素中所有满足条件p的元素。 |
| 16   | **def filterNot(p: (A) => Boolean): Iterator[A]**返回一个迭代器，指向迭代器元素中不满足条件p的元素。 |
| 17   | **def find(p: (A) => Boolean): Option[A]**返回第一个满足p的元素或None。注意：如果找到满足条件的元素，迭代器会被置于该元素之后；如果没有找到，会被置于终点。 |
| 18   | **def flatMap[B]\(f: (A) => GenTraversableOnce[B]): Iterator[B]**针对迭代器的序列中的每个元素应用函数f，并返回指向结果序列的迭代器。 |
| 19   | **def forall(p: (A) => Boolean): Boolean**返回一个布尔值，指明 it 所指元素是否都满足p。 |
| 20   | **def foreach(f: (A) => Unit): Unit**在迭代器返回的每个元素上执行指定的程序 f |
| 21   | **def hasDefiniteSize: Boolean**如果迭代器的元素个数有限则返回 true（默认等同于 isEmpty） |
| 22   | **def indexOf(elem: B): Int**返回迭代器的元素中index等于x的第一个元素。注意：迭代器会越过这个元素。 |
| 23   | **def indexWhere(p: (A) => Boolean): Int**返回迭代器的元素中下标满足条件p的元素。注意：迭代器会越过这个元素。 |
| 24   | **def isEmpty: Boolean**检查it是否为空, 为空返回 true，否则返回false（与hasNext相反）。 |
| 25   | **def isTraversableAgain: Boolean**Tests whether this Iterator can be repeatedly traversed. |
| 26   | **def length: Int**返回迭代器元素的数量。                    |
| 27   | **def map[B](f: (A) => B): Iterator[B]**将 it 中的每个元素传入函数 f 后的结果生成新的迭代器。 |
| 28   | **def max: A**返回迭代器迭代器元素中最大的元素。             |
| 29   | **def min: A**返回迭代器迭代器元素中最小的元素。             |
| 30   | **def mkString: String**将迭代器所有元素转换成字符串。       |
| 31   | **def mkString(sep: String): String**将迭代器所有元素转换成字符串，并指定分隔符。 |
| 32   | **def nonEmpty: Boolean**检查容器中是否包含元素（相当于 hasNext）。 |
| 33   | **def padTo(len: Int, elem: A): Iterator[A]**首先返回迭代器所有元素，追加拷贝 elem 直到长度达到 len。 |
| 34   | **def patch(from: Int, patchElems: Iterator[B], replaced: Int): Iterator[B]**返回一个新迭代器，其中自第 from 个元素开始的 replaced 个元素被迭代器所指元素替换。 |
| 35   | **def product: A**返回迭代器所指数值型元素的积。             |
| 36   | **def sameElements(that: Iterator[_]): Boolean**判断迭代器和指定的迭代器参数是否依次返回相同元素 |
| 37   | **def seq: Iterator[A]**返回集合的系列视图                   |
| 38   | **def size: Int**返回迭代器的元素数量                        |
| 39   | **def slice(from: Int, until: Int): Iterator[A]**返回一个新的迭代器，指向迭代器所指向的序列中从开始于第 from 个元素、结束于第 until 个元素的片段。 |
| 40   | **def sum: A**返回迭代器所指数值型元素的和                   |
| 41   | **def take(n: Int): Iterator[A]**返回前 n 个元素的新迭代器。 |
| 42   | **def toArray: Array[A]**将迭代器指向的所有元素归入数组并返回。 |
| 43   | **def toBuffer: Buffer[B]**将迭代器指向的所有元素拷贝至缓冲区 Buffer。 |
| 44   | **def toIterable: Iterable[A]**Returns an Iterable containing all elements of this traversable or iterator. This will not terminate for infinite iterators. |
| 45   | **def toIterator: Iterator[A]**把迭代器的所有元素归入一个Iterator容器并返回。 |
| 46   | **def toList: List[A]**把迭代器的所有元素归入列表并返回      |
| 47   | **def toMap[T, U]: Map[T, U]**将迭代器的所有键值对归入一个Map并返回。 |
| 48   | **def toSeq: Seq[A]**将代器的所有元素归入一个Seq容器并返回。 |
| 49   | **def toString(): String**将迭代器转换为字符串               |
| 50   | **def zip[B]\(that: Iterator[B]): Iterator[(A, B)**返回一个新迭代器，指向分别由迭代器和指定的迭代器 that 元素一一对应而成的二元组序列 |





