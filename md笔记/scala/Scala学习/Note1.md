- ## Scala 特性

    ### 面向对象特性

    Scala是一种纯面向对象的语言，每个值都是对象。对象的数据类型以及行为由类和特质描述。

    类抽象机制的扩展有两种途径：一种途径是子类继承，另一种途径是灵活的混入机制。这两种途径能避免多重继承的种种问题。

    ### 函数式编程

    Scala也是一种函数式语言，其函数也能当成值来使用。Scala提供了轻量级的语法用以定义匿名函数，支持高阶函数，允许嵌套多层函数，并支持柯里化。Scala的case class及其内置的模式匹配相当于函数式编程语言中常用的代数类型。

    更进一步，程序员可以利用Scala的模式匹配，编写类似正则表达式的代码处理XML数据。

    ### 静态类型

    Scala具备类型系统，通过编译时检查，保证代码的安全性和一致性。类型系统具体支持以下特性：

    - 泛型类
    - 协变和逆变
    - 标注
    - 类型参数的上下限约束
    - 把类别和抽象类型作为对象成员
    - 复合类型
    - 引用自己时显式指定类型
    - 视图
    - 多态方法

    ### 扩展性

    Scala的设计秉承一项事实，即在实践中，某个领域特定的应用程序开发往往需要特定于该领域的语言扩展。Scala提供了许多独特的语言机制，可以以库的形式轻易无缝添加新的语言结构：

    - 任何方法可用作前缀或后缀操作符
    - 可以根据预期类型自动构造闭包。

    ### 并发性

    Scala使用`Actor`作为其并发模型，`Actor`是类似线程的实体，通过邮箱发收消息。`Actor`可以复用线程，因此可以在程序中可以使用数百万个`Actor`,而线程只能创建数千个。在2.10之后的版本中，使用Akka作为其默认`Actor`实现。



## Scala Web 框架

以下列出了两个目前比较流行的 Scala 的 Web应用框架：

- [Lift 框架](http://liftweb.net/)
- [Play 框架](http://www.playframework.org/)





# Scala 基础语法



- **对象 -** 对象有属性和行为。例如：一只狗的状属性有：颜色，名字，行为有：叫、跑、吃等。对象是一个类的实例。
- **类 -** 类是对象的抽象，而对象是类的具体实例。
- **方法 -** 方法描述的基本的行为，一个类可以包含多个方法。
- **字段 -** 每个对象都有它唯一的实例变量集合，即字段。对象的属性通过给字段赋值来创建。



## 基本语法

Scala 基本语法需要注意以下几点：

- **区分大小写** -  Scala是大小写敏感的，这意味着标识Hello 和 hello在Scala中会有不同的含义。

- **类名** - 对于所有的类名的第一个字母要大写。
    如果需要使用几个单词来构成一个类的名称，每个单词的第一个字母要大写。

    示例：*class MyFirstScalaClass*

- **方法名称** - 所有的方法名称的第一个字母用小写。
    如果若干单词被用于构成方法的名称，则每个单词的第一个字母应大写。

    示例：*def myMethodName()*

- **程序文件名** - 程序文件的名称应该与对象名称完全匹配(新版本不需要了，但建议保留这种习惯)。
    保存文件时，应该保存它使用的对象名称（记住Scala是区分大小写），并追加".scala"为文件扩展名。 （如果文件名和对象名称不匹配，程序将无法编译）。

    示例: 假设"HelloWorld"是对象的名称。那么该文件应保存为'HelloWorld.scala"

- **def main(args: Array[String])** - Scala程序从main()方法开始处理，这是每一个Scala程序的强制程序入口部分。



***标识符是用户编程时使用的名字，用于给变量、[常量](https://baike.baidu.com/item/常量/10232375)、[函数](https://baike.baidu.com/item/函数/18686609)、语句块等命名，以建立起名称与使用之间的关系***



你可以在"之间使用任何有效的 Scala 标志符，Scala 将它们解释为一个 Scala 标志符，一个典型的使用为 Thread 的 yield 方法， 在 Scala 中你不能使用 Thread.yield()是因为 yield 为 Scala 中的关键字， 你必须使用

```scala
 Thread.`yield`()
```

来使用这个方法。



## 换行符

Scala是面向行的语言，语句可以用分号（;）结束或换行符。Scala 程序里,语句末尾的分号通常是可选的。如果你愿意可以输入一个,但若一行里仅 有一个语句也可不写。另一方面,如果一行里写多个语句那么分号是需要的。例如

```scala
val s = "菜鸟教程"; println(s)
```





## Scala 包

### 定义包

Scala 使用 package 关键字定义包，在Scala将代码定义到某个包中有两种方式：

第一种方法和 Java 一样，在文件的头定义包名，这种方法就后续所有代码都放在该包中。 比如：

```scala
package com.runoob
class HelloWorld
```

第二种方法有些类似 C#，如：

```scala
package com.runoob {
  class HelloWorld 
}
```

第二种方法，可以在一个文件中定义多个包。



### 引用

Scala 使用 import 关键字引用包。

```scala
import java.awt.Color  // 引入Color
 
import java.awt._  // 引入包内所有成员
 
def handler(evt: event.ActionEvent) { // java.awt.event.ActionEvent
  ...  // 因为引入了java.awt，所以可以省去前面的部分
}
```

import语句可以出现在任何地方，而不是只能在文件顶部。import的效果从开始延伸到语句块的结束。这可以大幅减少名称冲突的可能性。

如果想要引入包中的几个成员，可以使用selector（选取器）：

```scala
import java.awt.{Color, Font}
 
// 重命名成员
import java.util.{HashMap => JavaHashMap}
 
// 隐藏成员
import java.util.{HashMap => _, _} // 引入了util包的所有成员，但是HashMap被隐藏了
```

**注意：***默认情况下，Scala 总会引入 java.lang._ 、 scala._ 和 Predef._，这里也能解释，为什么以scala开头的包，在使用时都是省去scala.的。*





# Scala 数据类型

Scala 与 Java有着相同的数据类型，下表列出了 Scala 支持的数据类型：

| 数据类型 | 描述                                                         |
| :------- | :----------------------------------------------------------- |
| Byte     | 8位有符号补码整数。数值区间为 -128 到 127                    |
| Short    | 16位有符号补码整数。数值区间为 -32768 到 32767               |
| Int      | 32位有符号补码整数。数值区间为 -2147483648 到 2147483647     |
| Long     | 64位有符号补码整数。数值区间为 -9223372036854775808 到 9223372036854775807 |
| Float    | 32 位, IEEE 754 标准的单精度浮点数                           |
| Double   | 64 位 IEEE 754 标准的双精度浮点数                            |
| Char     | 16位无符号Unicode字符, 区间值为 U+0000 到 U+FFFF             |
| String   | 字符序列                                                     |
| Boolean  | true或false                                                  |
| Unit     | 表示无值，和其他语言中void等同。用作不返回任何结果的方法的结果类型。Unit只有一个实例值，写成()。 |
| Null     | null 或空引用                                                |
| Nothing  | Nothing类型在Scala的类层级的最底端；它是任何其他类型的子类型。 |
| Any      | Any是所有其他类的超类                                        |
| AnyRef   | AnyRef类是Scala里所有引用类(reference class)的基类           |

上表中列出的数据类型都是对象，也就是说scala没有java中的原生类型。在scala是可以对数字等基础类型调用方法的。



### 符号字面量

符号字面量被写成： **'<标识符>** ，这里 **<标识符>** 可以是任何字母或数字的标识（注意：不能以数字开头）。这种字面量被映射成预定义类scala.Symbol的实例。

如： 符号字面量 **'x** 是表达式 **scala.Symbol("x")** 的简写，符号字面量定义如下：



```scala
package scala
final case class Symbol private (name: String) {
   override def toString: String = "'" + name
}
```



### 字符字面量

在 Scala 字符变量使用单引号 **'** 来定义，如下：

```scala
'a' 
'\u0041'
'\n'
'\t'
```

其中 **\** 表示转义字符，其后可以跟 **u0041** 数字或者 **\r\n** 等固定的转义字符。



### 字符串字面量

在 Scala 字符串字面量使用双引号 **"** 来定义，如下：

```scala
"Hello,\nWorld!"
"菜鸟教程官网：www.runoob.com"
```



### 多行字符串的表示方法

多行字符串用三个双引号来表示分隔符，格式为：**""" ... """**。

实例如下：

```scala
val foo = """菜鸟教程
www.runoob.com
www.w3cschool.cc
www.runnoob.com
以上三个地址都能访问"""
```



### Null 值

空值是 scala.Null 类型。

Scala.Null和scala.Nothing是用统一的方式处理Scala面向对象类型系统的某些"边界情况"的特殊类型。

Null类是null引用对象的类型，它是每个引用类（继承自AnyRef的类）的子类。Null不兼容值类型。



### Scala 转义字符

下表列出了常见的转义字符：

| 转义字符 | Unicode | 描述                                |
| :------- | :------ | :---------------------------------- |
| \b       | \u0008  | 退格(BS) ，将当前位置移到前一列     |
| \t       | \u0009  | 水平制表(HT) （跳到下一个TAB位置）  |
| \n       | \u000a  | 换行(LF) ，将当前位置移到下一行开头 |
| \f       | \u000c  | 换页(FF)，将当前位置移到下页开头    |
| \r       | \u000d  | 回车(CR) ，将当前位置移到本行开头   |
| \"       | \u0022  | 代表一个双引号(")字符               |
| \'       | \u0027  | 代表一个单引号（'）字符             |
| \\       | \u005c  | 代表一个反斜线字符 '\'              |



##变量和常量

- 一、变量： 在程序运行过程中其值可能发生改变的量叫做变量。如：时间，年龄。
- 二、常量 在程序运行过程中其值不会发生变化的量叫做常量。如：数值 3，字符'A'。

在 Scala 中，使用关键词 **"var"** 声明变量，使用关键词 **"val"** 声明常量。

声明变量实例如下：

```scala
var myVar : String = "Foo"
var myVar : String = "Too"
```

以上定义了变量 myVar，我们可以修改它。

声明常量实例如下：

```scala
val myVal : String = "Foo"
```



## 变量类型声明

变量的类型在变量名之后等号之前声明。定义变量的类型的语法格式如下：

```scala
var VariableName : DataType [=  Initial Value]

//或

val VariableName : DataType [=  Initial Value]
```



## 变量类型引用

在 Scala 中声明变量和常量不一定要指明数据类型，在没有指明数据类型的情况下，其数据类型是通过变量或常量的初始值推断出来的。

所以，如果在没有指明数据类型的情况下声明变量或常量必须要给出其初始值，否则将会报错。

```scala
var myVar = 10;
val myVal = "Hello, Scala!";
```



# Scala 访问修饰符

Scala 访问修饰符基本和Java的一样，分别有：private，protected，public。

如果没有指定访问修饰符，默认情况下，Scala 对象的访问级别都是 public。

Scala 中的 private 限定符，比 Java 更严格，***在嵌套类情况下，外层类甚至不能访问被嵌套类的私有成员***。



## 私有(Private)成员

用 private 关键字修饰，带有此标记的成员仅在包含了成员定义的类或对象内部可见，同样的规则还适用内部类。

```scala
class Outer{
    class Inner{
    private def f(){println("f")}
    class InnerMost{
        f() // 正确
        }
    }
    (new Inner).f() //错误
}
```

**(new Inner).f( )** 访问不合法是因为 **f** 在 Inner 中被声明为 private，而访问不在类 Inner 之内。

但在 InnerMost 里访问 **f** 就没有问题的，因为这个访问包含在 Inner 类之内。

Java中允许这两种访问，因为它允许外部类访问内部类的私有成员。



## 保护(Protected)成员

在 scala 中，对保护（Protected）成员的访问比 java 更严格一些。因为它只允许保护成员在定义了该成员的的类的子类中被访问。而在java中，用protected关键字修饰的成员，除了定义了该成员的类的子类可以访问，同一个包里的其他类也可以进行访问。

```scala
package p{
class Super{
    protected def f() {println("f")}
    }
        class Sub extends Super{
            f()
        }
        class Other{
                (new Super).f() //错误
        }
}
```

上例中，Sub 类对 f 的访问没有问题，因为 f 在 Super 中被声明为 protected，而 Sub 是 Super 的子类。相反，Other 对 f 的访问不被允许，因为 other 没有继承自 Super。而后者在 java 里同样被认可，因为 Other 与 Sub 在同一包里。



## 作用域保护

Scala中，访问修饰符可以通过使用限定词强调。格式为:

```scala
private[x] 

protected[x]
```

这里的x指代某个所属的包、类或单例对象。如果写成private[x],读作"这个成员除了对[…]中的类或[…]中的包中的类及它们的伴生对像可见外，对其它所有类都是private。

```scala
package bobsrockets {
    package navigation {
        private[bobsrockets] class Navigator {
            protected[navigation] def useStarChart() {}

            class LegOfJourney {
                private[Navigator] val distance = 100
            }

            private[this] var speed = 200
        }
    }

    package launch {

        import navigation._

        object Vehicle {
            private[launch] val guide = new Navigator
        }
    }

}
```

上述例子中，类 Navigator 被标记为 private[bobsrockets] 就是说这个类对包含在 bobsrockets 包里的所有的类和对象可见。

比如说，从 Vehicle 对象里对 Navigator 的访问是被允许的，因为对象 Vehicle 包含在包 launch 中，而 launch 包在 bobsrockets 中，相反，所有在包 bobsrockets 之外的代码都不能访问类 Navigator。



# Scala 运算符

一个运算符是一个符号，用于告诉编译器来执行指定的数学运算和逻辑运算。

Scala 含有丰富的内置运算符，包括以下几种类型：

- 算术运算符
- 关系运算符
- 逻辑运算符
- 位运算符
- 赋值运算符



## 算术运算符

| 运算符 | 描述 | 实例                 |
| :----- | :--- | :------------------- |
| +      | 加号 | A + B 运算结果为 30  |
| -      | 减号 | A - B 运算结果为 -10 |
| *      | 乘号 | A * B 运算结果为 200 |
| /      | 除号 | B / A 运算结果为 2   |
| %      | 取余 | B % A 运算结果为 0   |



## 关系运算符

| 运算符 | 描述     | 实例                      |
| :----- | :------- | :------------------------ |
| ==     | 等于     | (A == B) 运算结果为 false |
| !=     | 不等于   | (A != B) 运算结果为 true  |
| >      | 大于     | (A > B) 运算结果为 false  |
| <      | 小于     | (A < B) 运算结果为 true   |
| >=     | 大于等于 | (A >= B) 运算结果为 false |
| <=     | 小于等于 | (A <= B) 运算结果为 true  |



## 逻辑运算符

| 运算符 | 描述   | 实例                       |
| :----- | :----- | :------------------------- |
| &&     | 逻辑与 | (A && B) 运算结果为 false  |
| \|\|   | 逻辑或 | (A \|\| B) 运算结果为 true |
| !      | 逻辑非 | !(A && B) 运算结果为 true  |



## 位运算符

位运算符用来对二进制位进行操作，**~,&,|,^** 分别为取反，按位与与，按位与或，按位与异或运算，如下表实例：

| p    | q    | p & q | p \| q | p ^ q |
| :--- | :--- | :---- | :----- | :---- |
| 0    | 0    | 0     | 0      | 0     |
| 0    | 1    | 0     | 1      | 1     |
| 1    | 1    | 1     | 1      | 0     |
| 1    | 0    | 0     | 1      | 1     |



Scala 中的按位运算法则如下：

| 运算符 | 描述           | 实例                                                         |
| :----- | :------------- | :----------------------------------------------------------- |
| &      | 按位与运算符   | (a & b) 输出结果 12 ，二进制解释： 0000 1100                 |
| \|     | 按位或运算符   | (a \| b) 输出结果 61 ，二进制解释： 0011 1101                |
| ^      | 按位异或运算符 | (a ^ b) 输出结果 49 ，二进制解释： 0011 0001                 |
| ~      | 按位取反运算符 | (~a ) 输出结果 -61 ，二进制解释： 1100 0011， 在一个有符号二进制数的补码形式。 |
| <<     | 左移动运算符   | a << 2 输出结果 240 ，二进制解释： 1111 0000                 |
| >>     | 右移动运算符   | a >> 2 输出结果 15 ，二进制解释： 0000 1111                  |
| >>>    | 无符号右移     | A >>>2 输出结果 15, 二进制解释: 0000 1111                    |



## 赋值运算符

以下列出了 Scala 语言支持的赋值运算符:

| 运算符 | 描述                                                         | 实例                                  |
| :----- | :----------------------------------------------------------- | :------------------------------------ |
| =      | 简单的赋值运算，指定右边操作数赋值给左边的操作数。           | C = A + B 将 A + B 的运算结果赋值给 C |
| +=     | 相加后再赋值，将左右两边的操作数相加后再赋值给左边的操作数。 | C += A 相当于 C = C + A               |
| -=     | 相减后再赋值，将左右两边的操作数相减后再赋值给左边的操作数。 | C -= A 相当于 C = C - A               |
| *=     | 相乘后再赋值，将左右两边的操作数相乘后再赋值给左边的操作数。 | C *= A 相当于 C = C * A               |
| /=     | 相除后再赋值，将左右两边的操作数相除后再赋值给左边的操作数。 | C /= A 相当于 C = C / A               |
| %=     | 求余后再赋值，将左右两边的操作数求余后再赋值给左边的操作数。 | C %= A is equivalent to C = C % A     |
| <<=    | 按位左移后再赋值                                             | C <<= 2 相当于 C = C << 2             |
| >>=    | 按位右移后再赋值                                             | C >>= 2 相当于 C = C >> 2             |
| &=     | 按位与运算后赋值                                             | C &= 2 相当于 C = C & 2               |
| ^=     | 按位异或运算符后再赋值                                       | C ^= 2 相当于 C = C ^ 2               |
| \|=    | 按位或运算后再赋值                                           | C \|= 2 相当于 C = C \| 2             |



##运算符

查看以下表格，优先级从上到下依次递减，最上面具有最高的优先级，逗号操作符具有最低的优先级。

| 类别 |               运算符               | 关联性 |
| :--: | :--------------------------------: | :----: |
|  1   |               () []                | 左到右 |
|  2   |                ! ~                 | 右到左 |
|  3   |               * / %                | 左到右 |
|  4   |                + -                 | 左到右 |
|  5   |             >> >>> <<              | 左到右 |
|  6   |             > >= < <=              | 左到右 |
|  7   |               == !=                | 左到右 |
|  8   |                 &                  | 左到右 |
|  9   |                 ^                  | 左到右 |
|  10  |                 \|                 | 左到右 |
|  11  |                 &&                 | 左到右 |
|  12  |                \|\|                | 左到右 |
|  13  | = += -= *= /= %= >>= <<= &= ^= \|= | 右到左 |
|  14  |                 ,                  | 左到右 |





# 循环和条件 - 类比 JAVA



## 循环类型

Scala 语言提供了以下几种循环类型。点击链接查看每个类型的细节。

| 循环类型                                                     | 描述                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [while 循环](https://www.runoob.com/scala/scala-while-loop.html) | 运行一系列语句，如果条件为true，会重复运行，直到条件变为false。 |
| [do...while 循环](https://www.runoob.com/scala/scala-do-while-loop.html) | 类似 while 语句区别在于判断循环条件之前，先执行一次循环的代码块。 |
| [for 循环](https://www.runoob.com/scala/scala-for-loop.html) | 用来重复执行一系列语句直到达成特定条件达成，一般通过在每次循环完成后增加计数器的值来实现。 |



## if...else 语句

if 语句后可以紧跟 else 语句，else 内的语句块可以在布尔表达式为 false 的时候执行。

### 语法

if...else 的语法格式如下：

```scala
if(布尔表达式){
   // 如果布尔表达式为 true 则执行该语句块
}else{
   // 如果布尔表达式为 false 则执行该语句块
}
```





# Scala 方法与函数



Scala 中的方法跟 Java 的类似，方法是组成类的一部分。

Scala 中的函数则是一个完整的对象，Scala 中的函数其实就是继承了 Trait 的类的对象。



Scala 中使用 **val** 语句可以定义函数，**def** 语句定义方法。

```scala
class Test{
  def m(x: Int) = x + 3
  val f = (x: Int) => x + 3
}
```



## 方法声明

Scala 方法声明格式如下：

```scala
def functionName ([参数列表]) : [return type]
```

## 方法定义

方法定义由一个 **def** 关键字开始，紧接着是可选的参数列表，一个冒号 **:** 和方法的返回类型，一个等于号 **=** ，最后是方法的主体。

Scala 方法定义格式如下：

```scala
def functionName ([参数列表]) : [return type] = {
   function body
   return [expr]
}
```



***Demo:***

```scala
object add{
   def addInt( a:Int, b:Int ) : Int = {
      var sum:Int = 0
      sum = a + b

      return sum
   }
}
```



## 方法调用

Scala 提供了多种不同的方法调用方式：

以下是调用方法的标准格式：

```scala
functionName( 参数列表 )
```

如果方法使用了实例的对象来调用，我们可以使用类似java的格式 (使用 **.** 号)：

```scala
[instance.]functionName( 参数列表 )
```

以上实例演示了定义与调用方法的实例:

## 实例

```scala
object Test {
  def main(args: Array[String]) {
    println( "Returned Value : " + addInt(5,7) );
  }
  def addInt( a:Int, b:Int ) : Int = {
   var sum:Int = 0
   sum = a + b

   return sum
  }
}
```



# Scala 闭包

闭包是一个函数，返回值依赖于声明在函数外部的一个或多个变量。

闭包通常来讲可以简单的认为是可以访问一个函数里面局部变量的另外一个函数。

如下面这段匿名的函数：

```scala
val multiplier = (i:Int) => i * 10  
```

函数体内有一个变量 i，它作为函数的一个参数。如下面的另一段代码：

```scala
val multiplier = (i:Int) => i * factor
```

在 multiplier 中有两个变量：i 和 factor。其中的一个 i 是函数的形式参数，在 multiplier 函数被调用时，i 被赋予一个新的值。然而，factor不是形式参数，而是自由变量，考虑下面代码：

```scala
var factor = 3  
val multiplier = (i:Int) => i * factor
```



这里我们引入一个自由变量 factor，这个变量定义在函数外面。

这样定义的函数变量 multiplier 成为一个"闭包"，因为它引用到函数外面定义的变量，定义这个函数的过程是将这个自由变量捕获而构成一个封闭的函数。

完整实例

## 实例

```scala
object Test {  
   def main(args: Array[String]) {  
      println( "muliplier(1) value = " +  multiplier(1) )  
      println( "muliplier(2) value = " +  multiplier(2) )  
   }  
   var factor = 3  
   val multiplier = (i:Int) => i * factor  
}
```

执行以上代码，输出结果为：

```
$ scalac Test.scala  
$  scala Test  
muliplier(1) value = 3  
muliplier(2) value = 6  
```



