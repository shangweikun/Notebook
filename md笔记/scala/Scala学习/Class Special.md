# Scala 类和对象

类是对象的抽象，而对象是类的具体实例。类是抽象的，不占用内存，而对象是具体的，占用存储空间。类是用于创建对象的蓝图，它是一个定义包括在特定类型的对象中的方法和变量的软件模板。

我们可以使用 new 关键字来创建类的对象，实例如下：

```scala
class Point(xc: Int, yc: Int) {
   var x: Int = xc
   var y: Int = yc

   def move(dx: Int, dy: Int) {
      x = x + dx
      y = y + dy
      println ("x 的坐标点: " + x);
      println ("y 的坐标点: " + y);
   }
}
```



Scala中的类不声明为public，一个Scala源文件中可以有多个类。

以上实例的类定义了两个变量 **x** 和 **y** ，一个方法：**move**，方法没有返回值。

Scala 的类定义可以有参数，称为类参数，如上面的 xc, yc，类参数在整个类中都可以访问。

接着我们可以使用 new 来实例化类，并访问类中的方法和变量：

```scala
import java.io._

class Point(xc: Int, yc: Int) {
   var x: Int = xc
   var y: Int = yc

   def move(dx: Int, dy: Int) {
      x = x + dx
      y = y + dy
      println ("x 的坐标点: " + x);
      println ("y 的坐标点: " + y);
   }
}

object Test {
   def main(args: Array[String]) {
      val pt = new Point(10, 20);

      // 移到一个新的位置
      pt.move(10, 10);
   }
}
```



## Scala 继承

Scala继承一个基类跟Java很相似, 但我们需要注意以下几点：

- 1、重写一个非抽象方法必须使用override修饰符。
- 2、只有主构造函数才可以往基类的构造函数里写参数。
- 3、在子类中重写超类的抽象方法时，你不需要使用override关键字。

```scala
class Point(xc: Int, yc: Int) {
   var x: Int = xc
   var y: Int = yc

   def move(dx: Int, dy: Int) {
      x = x + dx
      y = y + dy
      println ("x 的坐标点: " + x);
      println ("y 的坐标点: " + y);
   }
}

class Location(override val xc: Int, override val yc: Int,
   val zc :Int) extends Point(xc, yc){
   var z: Int = zc

   def move(dx: Int, dy: Int, dz: Int) {
      x = x + dx
      y = y + dy
      z = z + dz
      println ("x 的坐标点 : " + x);
      println ("y 的坐标点 : " + y);
      println ("z 的坐标点 : " + z);
   }
}
```



Scala 使用 extends 关键字来继承一个类。实例中 Location 类继承了 Point 类。Point 称为父类(基类)，Location 称为子类。

**override val xc** 为重写了父类的字段。

继承会继承父类的所有属性和方法，Scala 只允许继承一个父类。

```scala
import java.io._

class Point(val xc: Int, val yc: Int) {
   var x: Int = xc
   var y: Int = yc
   def move(dx: Int, dy: Int) {
      x = x + dx
      y = y + dy
      println ("x 的坐标点 : " + x);
      println ("y 的坐标点 : " + y);
   }
}

class Location(override val xc: Int, override val yc: Int,
   val zc :Int) extends Point(xc, yc){
   var z: Int = zc

   def move(dx: Int, dy: Int, dz: Int) {
      x = x + dx
      y = y + dy
      z = z + dz
      println ("x 的坐标点 : " + x);
      println ("y 的坐标点 : " + y);
      println ("z 的坐标点 : " + z);
   }
}

object Test {
   def main(args: Array[String]) {
      val loc = new Location(10, 20, 15);

      // 移到一个新的位置
      loc.move(10, 10, 5);
   }
}
```



Scala重写一个非抽象方法，必须用override修饰符。

```scala
class Person {
  var name = ""
  override def toString = getClass.getName + "[name=" + name + "]"
}

class Employee extends Person {
  var salary = 0.0
  override def toString = super.toString + "[salary=" + salary + "]"
}

object Test extends App {
  val fred = new Employee
  fred.name = "Fred"
  fred.salary = 50000
  println(fred)
}
```



## Scala 单例对象

在 Scala 中，是没有 static 这个东西的，但是它也为我们提供了单例模式的实现方法，那就是使用关键字 object。

Scala 中使用单例模式时，除了定义的类之外，还要定义一个同名的 object 对象，它和类的区别是，object对象不能带参数。

当单例对象与某个类共享同一个名称时，他被称作是这个类的伴生对象：companion object。你必须在同一个源文件里定义类和它的伴生对象。类被称为是这个单例对象的伴生类：companion class。类和它的伴生对象可以互相访问其私有成员。

### 单例对象实例

```scala
**import** java.io._

**class** Point(**val** xc: Int, **val** yc: Int) {
  **var** x: Int = xc
  **var** y: Int = yc
  **def** move(dx: Int, dy: Int) {
   x = x + dx
   y = y + dy
  }
}

**object** Test {
  **def** main(args: Array[String]) {
   **val** point = **new** Point(10, 20)
   printPoint

   **def** printPoint{
     println ("x 的坐标点 : " + point.x);
     println ("y 的坐标点 : " + point.y);
   }
  }
}
```



### 伴生对象实例

```scala
// 私有构造方法
class Marker private(val color:String) {

  println("创建" + this)
 
  override def toString(): String = "颜色标记："+ color
 
}

// 伴生对象，与类名字相同，可以访问类的私有属性和方法
object Marker{
 
    private val markers: Map[String, Marker] = Map(
      "red" -> new Marker("red"),
      "blue" -> new Marker("blue"),
      "green" -> new Marker("green")
    )
   
    def apply(color:String) = {
      if(markers.contains(color)) markers(color) else null
    }
 
   
    def getMarker(color:String) = {
      if(markers.contains(color)) markers(color) else null
    }
    def main(args: Array[String]) {
        println(Marker("red"))  
        // 单例函数调用，省略了.(点)符号  
                println(Marker getMarker "blue")  
    }
}
```





# Scala Trait(特征)



Scala Trait(特征) 相当于 Java 的接口，实际上它比接口还功能强大。

与接口不同的是，它还可以定义属性和方法的实现。

一般情况下Scala的类只能够继承单一父类，但是如果是 Trait(特征) 的话就可以继承多个，从结果来看就是实现了多重继承。

Trait(特征) 定义的方式与类类似，但它使用的关键字是 **trait**



```scala
trait Equal {
  def isEqual(x: Any): Boolean
  def isNotEqual(x: Any): Boolean = !isEqual(x)
}
```

以上Trait(特征)由两个方法组成：**isEqual** 和 **isNotEqual**。isEqual 方法没有定义方法的实现，isNotEqual定义了方法的实现。子类继承特征可以实现未被实现的方法。所以其实 Scala Trait(特征)更像 Java 的抽象类。

具体的实例如下：

```scala
trait Equal {
  def isEqual(x: Any): Boolean
  def isNotEqual(x: Any): Boolean = !isEqual(x)
}

class Point(xc: Int, yc: Int) extends Equal {
  var x: Int = xc
  var y: Int = yc
  def isEqual(obj: Any) =
    obj.isInstanceOf[Point] &&
    obj.asInstanceOf[Point].x == x
}

object Test {
   def main(args: Array[String]) {
      val p1 = new Point(2, 3)
      val p2 = new Point(2, 4)
      val p3 = new Point(3, 3)

      println(p1.isNotEqual(p2))
      println(p1.isNotEqual(p3))
      println(p1.isNotEqual(2))
   }
}
```



## 特征构造顺序

特征也可以有构造器，由字段的初始化和其他特征体中的语句构成。这些语句在任何混入该特征的对象在构造时都会被执行。

构造器的执行顺序：

- 调用超类的构造器；
- 特征构造器在超类构造器之后、类构造器之前执行；
- 特征由左到右被构造；
- 每个特征当中，父特征先被构造；
- 如果多个特征共有一个父特征，父特征不会被重复构造
- 所有特征被构造完毕，子类被构造。

构造器的顺序是类的线性化的反向。线性化是描述某个类型的所有超类型的一种技术规格。





# Scala 模式匹配



Scala 提供了强大的模式匹配机制，应用也非常广泛。

一个模式匹配包含了一系列备选项，每个都开始于关键字 **case**。每个备选项都包含了一个模式及一到多个表达式。箭头符号 **=>** 隔开了模式和表达式。

```scala
object Test {
   def main(args: Array[String]) {
      println(matchTest(3))

   }
   def matchTest(x: Int): String = x match {
      case 1 => "one"
      case 2 => "two"
      case _ => "many"
   }
}
```



match 对应 Java 里的 switch，但是写在选择器表达式之后。即： **选择器 match {备选项}。**

match 表达式通过以代码编写的先后次序尝试每个模式来完成计算，只要发现有一个匹配的case，剩下的case不会继续匹配。

```scala
object Test {
   def main(args: Array[String]) {
      println(matchTest("two"))
      println(matchTest("test"))
      println(matchTest(1))
      println(matchTest(6))

   }
   def matchTest(x: Any): Any = x match {
      case 1 => "one"
      case "two" => 2
      case y: Int => "scala.Int"
      case _ => "many"
   }
}
```

实例中第一个 case 对应整型数值 1，第二个 case 对应字符串值 two，第三个 case 对应类型模式，用于判断传入的值是否为整型，相比使用isInstanceOf来判断类型，使用模式匹配更好。第四个 case 表示默认的全匹配备选项，即没有找到其他匹配时的匹配项，类似 switch 中的 default。



## 使用样例类

使用了case关键字的类定义就是样例类(case classes)，样例类是种特殊的类，经过优化以用于模式匹配。

```scala
object Test {
   def main(args: Array[String]) {
        val alice = new Person("Alice", 25)
        val bob = new Person("Bob", 32)
        val charlie = new Person("Charlie", 32)
   
    for (person <- List(alice, bob, charlie)) {
        person match {
            case Person("Alice", 25) => println("Hi Alice!")
            case Person("Bob", 32) => println("Hi Bob!")
            case Person(name, age) =>
               println("Age: " + age + " year, name: " + name + "?")
         }
      }
   }
   // 样例类
   case class Person(name: String, age: Int)
}
```

在声明样例类时，下面的过程自动发生了：

- 构造器的每个参数都成为val，除非显式被声明为var，但是并不推荐这么做；
- 在伴生对象中提供了apply方法，所以可以不使用new关键字就可构建对象；
- 提供unapply方法使模式匹配可以工作；
- 生成toString、equals、hashCode和copy方法，除非显示给出这些方法的定义。





# Scala 正则表达式

Scala 通过 scala.util.matching 包中的 **Regex** 类来支持正则表达式。

```scala
import scala.util.matching.Regex

object Test {
   def main(args: Array[String]) {
      val pattern = "Scala".r
      val str = "Scala is Scalable and cool"
     
      println(pattern findFirstIn str)
   }
}
```

实例中使用 String 类的 r() 方法构造了一个Regex对象。

然后使用 findFirstIn 方法找到首个匹配项。

如果需要查看所有的匹配项可以使用 findAllIn 方法。

你可以使用 mkString( ) 方法来连接正则表达式匹配结果的字符串，***并可以使用管道(|)来设置不同的模式***：

```scala
import scala.util.matching.Regex

object Test {
   def main(args: Array[String]) {
      val pattern = new Regex("(S|s)cala")  // 首字母可以是大写 S 或小写 s
      val str = "Scala is scalable and cool"
     
      println((pattern findAllIn str).mkString(","))   // 使用逗号 , 连接返回结果
   }
}
```



如果你需要将匹配的文本替换为指定的关键词，可以使用 **replaceFirstIn( )** 方法来替换第一个匹配项，使用 **replaceAllIn( )** 方法替换所有匹配项

```scala
object Test {
   def main(args: Array[String]) {
      val pattern = "(S|s)cala".r
      val str = "Scala is scalable and cool"
     
      println(pattern replaceFirstIn(str, "Java"))
   }
}
```

```shell
$ scalac Test.scala 
$ scala Test
Java is scalable and cool
```



## 正则表达式

Scala 的正则表达式继承了 Java 的语法规则，Java 则大部分使用了 Perl 语言的规则。

下表我们给出了常用的一些正则表达式规则：



| 表达式    | 匹配规则                                                     |
| :-------- | :----------------------------------------------------------- |
| ^         | 匹配输入字符串开始的位置。                                   |
| $         | 匹配输入字符串结尾的位置。                                   |
| .         | 匹配除"\r\n"之外的任何单个字符。                             |
| [...]     | 字符集。匹配包含的任一字符。例如，"[abc]"匹配"plain"中的"a"。 |
| [^...]    | 反向字符集。匹配未包含的任何字符。例如，"[^abc]"匹配"plain"中"p"，"l"，"i"，"n"。 |
| \\A       | 匹配输入字符串开始的位置（无多行支持）                       |
| \\z       | 字符串结尾(类似$，但不受处理多行选项的影响)                  |
| \\Z       | 字符串结尾或行尾(不受处理多行选项的影响)                     |
| re*       | 重复零次或更多次                                             |
| re+       | 重复一次或更多次                                             |
| re?       | 重复零次或一次                                               |
| re{ n}    | 重复n次                                                      |
| re{ n,}   |                                                              |
| re{ n, m} | 重复n到m次                                                   |
| a\|b      | 匹配 a 或者 b                                                |
| (re)      | 匹配 re,并捕获文本到自动命名的组里                           |
| (?: re)   | 匹配 re,不捕获匹配的文本，也不给此分组分配组号               |
| (?> re)   | 贪婪子表达式                                                 |
| \\w       | 匹配字母或数字或下划线或汉字                                 |
| \\W       | 匹配任意不是字母，数字，下划线，汉字的字符                   |
| \\s       | 匹配任意的空白符,相等于 [\t\n\r\f]                           |
| \\S       | 匹配任意不是空白符的字符                                     |
| \\d       | 匹配数字，类似 [0-9]                                         |
| \\D       | 匹配任意非数字的字符                                         |
| \\G       | 当前搜索的开头                                               |
| \\n       | 换行符                                                       |
| \\b       | 通常是单词分界位置，但如果在字符类里使用代表退格             |
| \\B       | 匹配不是单词开头或结束的位置                                 |
| \\t       | 制表符                                                       |
| \\Q       | 开始引号：**\Q(a+b)\*3\E** 可匹配文本 "(a+b)*3"。            |
| \\E       | 结束引号：**\Q(a+b)\*3\E** 可匹配文本 "(a+b)*3"。            |

------

## 正则表达式实例

| 实例            | 描述                                          |
| :-------------- | :-------------------------------------------- |
| .               | 匹配除"\r\n"之外的任何单个字符。              |
| [Rr]uby         | 匹配 "Ruby" 或 "ruby"                         |
| rub[ye]         | 匹配 "ruby" 或 "rube"                         |
| [aeiou]         | 匹配小写字母 ：aeiou                          |
| [0-9]           | 匹配任何数字，类似 [0123456789]               |
| [a-z]           | 匹配任何 ASCII 小写字母                       |
| [A-Z]           | 匹配任何 ASCII 大写字母                       |
| [a-zA-Z0-9]     | 匹配数字，大小写字母                          |
| [^aeiou]        | 匹配除了 aeiou 其他字符                       |
| [^0-9]          | 匹配除了数字的其他字符                        |
| \\d             | 匹配数字，类似: [0-9]                         |
| \\D             | 匹配非数字，类似: [^0-9]                      |
| \\s             | 匹配空格，类似: [ \t\r\n\f]                   |
| \\S             | 匹配非空格，类似: [^ \t\r\n\f]                |
| \\w             | 匹配字母，数字，下划线，类似: [A-Za-z0-9_]    |
| \\W             | 匹配非字母，数字，下划线，类似: [^A-Za-z0-9_] |
| ruby?           | 匹配 "rub" 或 "ruby": y 是可选的              |
| ruby*           | 匹配 "rub" 加上 0 个或多个的 y。              |
| ruby+           | 匹配 "rub" 加上 1 个或多个的 y。              |
| \\d{3}          | 刚好匹配 3 个数字。                           |
| \\d{3,}         | 匹配 3 个或多个数字。                         |
| \\d{3,5}        | 匹配 3 个、4 个或 5 个数字。                  |
| \\D\\d+         | 无分组： + 重复 \d                            |
| (\\D\\d)+/      | 分组： + 重复 \D\d 对                         |
| ([Rr]uby(, )?)+ | 匹配 "Ruby"、"Ruby, ruby, ruby"，等等         |

注意上表中的每个字符使用了两个反斜线。这是因为在 Java 和 Scala 中字符串中的反斜线是转义字符。所以如果你要输出 **\**，你需要在字符串中写成 **\\** 来获取一个反斜线。查看以下实例：





# Scala 异常处理



Scala 的异常处理和其它语言比如 Java 类似。

Scala 的方法可以通过抛出异常的方法的方式来终止相关代码的运行，不必通过返回值。



## 抛出异常

Scala 抛出异常的方法和 Java一样，使用 throw 方法，例如，抛出一个新的参数异常：

```scala
throw new IllegalArgumentException
```



## 捕获异常

异常捕捉的机制与其他语言中一样，如果有异常发生，catch 字句是按次序捕捉的。因此，在 catch 字句中，越具体的异常越要靠前，越普遍的异常越靠后。 如果抛出的异常不在 catch 字句中，该异常则无法处理，会被升级到调用者处。

捕捉异常的 catch 子句，语法与其他语言中不太一样。

```scala
import java.io.FileReader
import java.io.FileNotFoundException
import java.io.IOException

object Test {
   def main(args: Array[String]) {
      try {
         val f = new FileReader("input.txt")
      } catch {
         case ex: FileNotFoundException =>{
            println("Missing file exception")
         }
         case ex: IOException => {
            println("IO Exception")
         }
      }
   }
}
```

catch字句里的内容跟match里的case是完全一样的。由于异常捕捉是按次序，如果最普遍的异常，Throwable，写在最前面，则在它后面的case都捕捉不到，因此需要将它写在最后面。



## finally 语句

finally 语句用于执行不管是正常处理还是有异常发生时都需要执行的步骤

```scala
import java.io.FileReader
import java.io.FileNotFoundException
import java.io.IOException

object Test {
   def main(args: Array[String]) {
      try {
         val f = new FileReader("input.txt")
      } catch {
         case ex: FileNotFoundException => {
            println("Missing file exception")
         }
         case ex: IOException => {
            println("IO Exception")
         }
      } finally {
         println("Exiting finally...")
      }
   }
}
```





# Scala 提取器(Extractor)



提取器是从传递给它的对象中提取出构造该对象的参数。

Scala 标准库包含了一些预定义的提取器，我们会大致的了解一下它们。

Scala 提取器是一个带有unapply方法的对象。unapply方法算是apply方法的反向操作：unapply接受一个对象，然后从对象中提取值，提取的值通常是用来构造该对象的值。



```scala
object Test {
   def main(args: Array[String]) {
     
      println ("Apply 方法 : " + apply("Zara", "gmail.com"));
      println ("Unapply 方法 : " + unapply("Zara@gmail.com"));
      println ("Unapply 方法 : " + unapply("Zara Ali"));

   }
   // 注入方法 (可选)
   def apply(user: String, domain: String) = {
      user +"@"+ domain
   }

   // 提取方法（必选）
   def unapply(str: String): Option[(String, String)] = {
      val parts = str split "@"
      if (parts.length == 2){
         Some(parts(0), parts(1))
      }else{
         None
      }
   }
}
```

以上对象定义了两个方法： **apply** 和 **unapply** 方法。通过 apply 方法我们无需使用 new 操作就可以创建对象。所以你可以通过语句 Test("Zara", "gmail.com") 来构造一个字符串 "Zara@gmail.com"。

unapply方法算是apply方法的反向操作：unapply接受一个对象，然后从对象中提取值，提取的值通常是用来构造该对象的值。实例中我们使用 Unapply 方法从对象中提取用户名和邮件地址的后缀。

实例中 unapply 方法在传入的字符串不是邮箱地址时返回 None。

```shell
$ scalac Test.scala 
$ scala Test
Apply 方法 : Zara@gmail.com
Unapply 方法 : Some((Zara,gmail.com))
Unapply 方法 : None
```



## 提取器使用模式匹配

在我们实例化一个类的时，可以带上0个或者多个的参数，编译器在实例化的时会调用 apply 方法。我们可以在类和对象中都定义 apply 方法。

就像我们之前提到过的，unapply 用于提取我们指定查找的值，它与 apply 的操作相反。 当我们在提取器对象中使用 match 语句是，unapply 将自动执行，如下所示：

```scala
object Test {
   def main(args: Array[String]) {
     
      val x = Test(5)
      println(x)

      x match
      {
         case Test(num) => println(x + " 是 " + num + " 的两倍！")
         //unapply 被调用
         case _ => println("无法计算")
      }

   }
   def apply(x: Int) = x*2
   def unapply(z: Int): Option[Int] = if (z%2==0) Some(z/2) else None
}
```

输出：

```shell
$ scalac Test.scala 
$ scala Test
10
10 是 5 的两倍！
```





# Scala 文件 I/O



Scala 进行文件写操作，直接用的都是 java中 的 I/O 类 （**java.io.File**)：

```scala
import java.io._

object Test {
   def main(args: Array[String]) {
      val writer = new PrintWriter(new File("test.txt" ))

      writer.write("菜鸟教程")
      writer.close()
   }
}
```

执行以上代码，会在你的当前目录下生产一个 test.txt 文件，文件内容为"菜鸟教程":

```shell
$ scalac Test.scala 
$ scala Test
$ cat test.txt 
菜鸟教程
```



## 从屏幕上读取用户输入

```scala
import scala.io._
object Test {
   def main(args: Array[String]) {
      print("请输入菜鸟教程官网 : " )
      val line = StdIn.readLine()

      println("谢谢，你输入的是: " + line)
   }
}
```

> Scala2.11 后的版本 **Console.readLine** 已废弃，使用 **scala.io.StdIn.readLine()** 方法代替。



## 从文件上读取内容

从文件读取内容非常简单。我们可以使用 Scala 的 **Source** 类及伴生对象来读取文件。

```scala
import scala.io.Source

object Test {
   def main(args: Array[String]) {
      println("文件内容为:" )

      Source.fromFile("test.txt" ).foreach{
         print
      }
   }
}
```

