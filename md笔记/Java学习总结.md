# Java基础

## 一、基础类型

1. boolean / 0|1;

2. byte / 8bit / -127 ~ +127;
3. short / 16bit(2byte)  / -2^15 ~ +2^15;
4. int / 32bit(4byte) / -2^31 ~ +2^31;
5. long / 64bit(8byte) / -2^63 ~ +2^63;
6. char / 16bit(2byte) / 0 ~ 
7. float / 32bit
8. double / 64bit

**ps：局部变量区（与 操作数栈 一起组成 ‘JAVA解释栈桢’）**

​		**数据占据4byte[32bit HotSpot]（除：long和double 占据8byte）**



#### 封装类型

自动拆箱和自动装箱

使用了缓存池 Integer.valueOf	具体实现：

```java
public static Integer valueOf(int i) {
        if (i >= IntegerCache.low && i <= IntegerCache.high)
            return IntegerCache.cache[i + (-IntegerCache.low)];
        return new Integer(i);
    }
```



 * 缓存池

   从初始化的池中进行获取对象	例如：IntegerCache

   ```java
   private static class IntegerCache {
           static final int low = -128;
           static final int high;
           static final Integer cache[];
     
     			static{
             /*···见下···*/
           }
     
           private IntegerCache() {}
       }
   ```

   可参数配置上限大小；

   ```java
   static {
               // high value may be configured by property
               int h = 127;
               String integerCacheHighPropValue =
                   sun.misc.VM.getSavedProperty("java.lang.Integer.IntegerCache.high");
               if (integerCacheHighPropValue != null) {
                   try {
                       int i = parseInt(integerCacheHighPropValue);
                       i = Math.max(i, 127);
                       // Maximum array size is Integer.MAX_VALUE
                       h = Math.min(i, Integer.MAX_VALUE - (-low) -1);
                   } catch( NumberFormatException nfe) {
                       // If the property cannot be parsed into an int, ignore it.
                   }
               }
               high = h;
   
               cache = new Integer[(high - low) + 1];
               int j = low;
               for(int k = 0; k < cache.length; k++)
                   cache[k] = new Integer(j++);
   
               // range [-128, 127] must be interned (JLS7 5.1.7)
               assert IntegerCache.high >= 127;
           }
   ```

   在启动 jvm 的时候，通过 -XX:AutoBoxCacheMax=<size> 来指定这个缓冲池的大小，该选项在 JVM 初始化的时候会设定一个名为 java.lang.IntegerCache.high 系统属性，然后 IntegerCache 初始化的时候就会读取该系统属性来决定上界。





## 二、String

















































# Java容器及结构实现

### LinkedList

### ArrayList



#### 应用

基于ArrayList实现的LRU算法

1. key-value的对应关系；
2. size值表示当前最新使用的数据；
3. get( )时的倒序处理，保证迭代顺序是“从新到旧”

```java
class LRUCache {

        class Node {

            public int key;
            public int value;

            public Node(int key, int value) {

                this.key = key;
                this.value = value;
            }
        }

        List<Node> LRUDATA;
        final int capacity;
        int size;

        public LRUCache(int capacity) {

            this.LRUDATA = new ArrayList<>(capacity);
            this.capacity = capacity;
            this.size = -1;
        }

        public int get(int key) {
            if (this.size == -1) return -1;
            Node tmp = null;
            Node result = null;
            for (int i = this.size; i >= 0; i--) {
                if ((tmp = LRUDATA.get(i)).key != key) continue;
                LRUDATA.remove(i);
                LRUDATA.add(tmp);
                result = tmp;
                break;
            }
            return result == null ? -1 : result.value;
        }

        public void put(int key, int value) {

            if (get(key) != -1) {
                LRUDATA.remove(this.size);
                Node newNode = new Node(key, value);
                LRUDATA.add(newNode);
                return;
            }

            if (this.size == this.capacity - 1) {

                LRUDATA.remove(0);
                Node newNode = new Node(key, value);
                LRUDATA.add(newNode);
            } else if (this.size < this.capacity) {

                Node newNode = new Node(key, value);
                LRUDATA.add(newNode);
                this.size++;
            }
        }
    }
```









# jvm虚拟机

###- jvm基础

JRE java running environment  

一次编译,多次运行jvm机器; 



混合似的编译器: 

​    即时编译器 和 解释编译器    

​    运行时决定的 “虚方法” 来说,即使编译器更有效率, 

​    不需要多次对虚函数(多态化)解释; 



###- jvm类加载器：

####类加载的三大过程：

1. **加载**：双亲委派模型的加载模式（父类率先加载） - 扩展加载器和应用加载器；

   ​			对java的class文件查找，找到对应字节流，创建对应的类；

   ​			**ps：对与不同的类加载器，加载同一个文件字节流，JVM会识别为不同的类**

2. **链接：**分为下面三大步骤

   1. 验证：
   2. 准备：为静态字段分配内存空间；**同时生成符号引用？**
   3. 解析：将符号引用解析为对应的实际引用（**在这一阶段，如果引用类未加载，将触发类的“1.加载”**）

3. **初始化**: 对类的静态**“基本类型和字符串”**变量（final）（是否和Pool的缓存池有关）；

   ​			  以**🔒**方式执行 类的init( )的方法；

   八大初始化场景：

   1. jvm启动时的函数指定的类
   2. 当new 一个类的时候
   3. 当调用某个类的静态方法时
   4. 当引用某个类的静态变量时
   5. 子类触发父类的初始化
   6. 当通过反射API触发类加载时
   7. 一个接口定义了default方法，直接或间接初始化该类的实现时，会触发
   8. 当调用MethodHandle实例时，初始化，指向的类

启动类加载器(boot class loader)



### 反射的调用

####Inflation机制：

​			前15 次采用代理实现到本地代理实现的方式；之后会通过字节码方式，动态实现。

**ps：-Dsun.reflect.noInflation=true**



关闭后，Method.invoke 将导致**瓶颈**，jvm会记录不同的**动态实现**的调用点（profile）；

执行不同的profile将会导致，Method.invoke被干扰；





### java对象内存

64位虚拟机中：压缩指针   -XX:+UseCompressedOops

**内存对齐？**





#### hashcode和equals

hashcode时Object的区别值，equals的本质是 == 操作，地址相等操作

equals 为 true => hashcode相等 **反之不然**；

String等重写了equals方法，表示值相等。



###Jvm GC机制

可访达分析算法

GC root 简单理解：***堆外指向堆内的引用***

有且不仅限

1. Java 方法栈桢中的局部变量;  ***栈***

2. 已加载类的静态变量; ***非堆***
3. JNI handles; 
4.  已启动且未停止的 Java 线程。 ***栈***



Stop-The-World

safepoint



***JVM参数***

\- XX:+UsePSAdaptiveSurvivorSizePolicy   Eden区的动态分配参数

-XX:SurvivorRatio   Eden区的固定参数比例

-XX:+MaxTenuringThreshold   老年代晋升参数

-XX:+UseCondCardMark   老年代中的卡表减少写回操作

-------







### 小数点保留

1. java.text.DecimalFormat   df   =new   java.text.DecimalFormat("#.00"); 
   df.format(你要格式化的数字);

2. double d = 3.1415926;
   String result = String .format("%.2f");

3. NumberFormat ddf1=NumberFormat.getNumberInstance() ; 

   ddf1.setMaximumFractionDigits(2); 

4. ```java
   static class TT {
   			public static void main(String args[]) {
   				double x = 23.5455;
   				NumberFormat ddf1 = NumberFormat.getNumberInstance();
   
   				ddf1.setMaximumFractionDigits(2);
   				String s = ddf1.format(x);
   				System.out.print(s);
   			}
   		}
   ```

   





### JAVA正则表达式

\\\ - 表示转换符

Pattern.matches（pattern，str）





#Java concurrent相关细节

ThreadPoolExecutor

***runWorker*** 的finally代码块中将执行 ***processWorkerExit***

***tryTerminate*** 方法

中将唤起一个空闲线程进行中断
然后由这个线程来中断其他空闲线程

**递归下去**

```java
    private void processWorkerExit(Worker w, boolean completedAbruptly) {
        if (completedAbruptly) // If abrupt, then workerCount wasn't adjusted
            decrementWorkerCount();

        final ReentrantLock mainLock = this.mainLock;
        mainLock.lock();
        try {
            completedTaskCount += w.completedTasks;
            workers.remove(w);
        } finally {
            mainLock.unlock();
        }

        tryTerminate();

        int c = ctl.get();
        if (runStateLessThan(c, STOP)) {
            if (!completedAbruptly) {
                int min = allowCoreThreadTimeOut ? 0 : corePoolSize;
                if (min == 0 && ! workQueue.isEmpty())
                    min = 1;
                if (workerCountOf(c) >= min)
                    return; // replacement not needed
            }
            addWorker(null, false);
        }
    }
```

 





# Java正则

```java
System.out.println("你".matches("[\\u4E00-\\u9FA5]*"));
System.out.println(("\n").matches("\\n"));
System.out.println(("1").matches("\\d"));

System.out.println(("a12").matches("a\\d"));  //false	-	12算作是两个字符
System.out.println(("a12").matches("a\\d*"));  //true	

System.out.println(("18369185667").matches("1[0-9]{10,10}"));

String regex = "\\b[a|b|c|d|e]\\b";
Pattern pattern = Pattern.compile(regex);
Matcher matcher = pattern.matcher("a b f c d e f");
```

匹配中文字符

```
\\ 转译对应的特殊字符 \\( 表示“左括号”

```

\b 会匹配字符的边界 （空格和字，开始、结尾和字）





#java注解类

《On Java 8》



### [定义注解](https://lingcoder.github.io/OnJava8/#/book/23-Annotations?id=定义注解)

​		如下是一个注解的定义。注解的定义看起来很像接口的定义。事实上，它们和其他 Java 接口一样，也会被编译成 class 文件。

```java
// onjava/atunit/Test.java
// The @Test tag
package onjava.atunit;
import java.lang.annotation.*;
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface Test {}
```

​		注解的定义也需要一些元注解（meta-annotation），比如 `@Target` 和 `@Retention`。`@Target` 定义你的注解可以应用在哪里（例如是方法还是字段）。`@Retention` 定义了注解在哪里可用，在源代码中（SOURCE），class文件（CLASS）中或者是在运行时（RUNTIME）。



​		下面是一个简单的注解，我们可以用它来追踪项目中的用例。程序员可以使用该注解来标注满足特定用例的一个方法或者一组方法。于是，项目经理可以通过统计已经实现的用例来掌控项目的进展，而开发者在维护项目时可以轻松的找到用例用于更新，或者他们可以调试系统中业务逻辑。

```java
// annotations/UseCase.java
import java.lang.annotation.*;
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface UseCase {
    int id();
    String description() default "no description";
}
```



### [元注解](https://lingcoder.github.io/OnJava8/#/book/23-Annotations?id=元注解)

Java 语言中目前有 5 种标准注解（前面介绍过），以及 5 种元注解。元注解用于注解其他的注解

| 注解        | 解释                                                         |
| ----------- | ------------------------------------------------------------ |
| @Target     | 表示注解可以用于哪些地方。可能的 **ElementType** 参数包括： **CONSTRUCTOR**：构造器的声明 **FIELD**：字段声明（包括 enum 实例） **LOCAL_VARIABLE**：局部变量声明 **METHOD**：方法声明 **PACKAGE**：包声明 **PARAMETER**：参数声明 **TYPE**：类、接口（包括注解类型）或者 enum 声明 |
| @Retention  | 表示注解信息保存的时长。可选的 **RetentionPolicy** 参数包括： **SOURCE**：注解将被编译器丢弃 **CLASS**：注解在 class 文件中可用，但是会被 VM 丢弃。 **RUNTIME**：VM 将在运行期也保留注解，因此可以通过反射机制读取注解的信息。 |
| @Documented | 将此注解保存在 Javadoc 中                                    |
| @Inherited  | 允许子类继承父类的注解                                       |
| @Repeatable | 允许一个注解可以被使用一次或者多次（Java 8）。               |



两个反射的方法：`getDeclaredMethods()` 和 `getAnnotation()`，它们都属于 **AnnotatedElement** 接口（**Class**，**Method** 与 **Field** 类都实现了该接口）。`getAnnotation()` 方法返回指定类型的注解对象，在本例中就是 “**UseCase**”。如果被注解的方法上没有该类型的注解，返回值就为 **null**。我们通过调用 `id()` 和 `description()` 方法来提取元素值。注意 `encryptPassword()` 方法在注解的时候没有指定 **description** 的值，因此处理器在处理它对应的注解时，通过 `description()` 取得的是默认值 “no description”。

`getDeclaredMethods()`：可以所有声明的方法；

​				相对于`getMethods()`，只能获取类内声明的所有方法，对于继承的Object或其他超类方法并不能获取；但`getMethods()`只能获取所有的public声明方法，相对来说还是有区别，有不同的使用场景；

ps：utility classes in java

**Utility Class**, also known as **Helper class**, is a **class**, which contains just static methods, it is stateless and cannot be instantiated. It contains a bunch of related methods, so they can be reused across the application. As an example consider Apache StringUtils, CollectionUtils or **java**. lang.



### [注解元素](https://lingcoder.github.io/OnJava8/#/book/23-Annotations?id=注解元素)

在 **UseCase.java** 中定义的 **@UseCase** 的标签包含 int 元素 **id** 和 String 元素 **description**。注解元素可用的类型如下所示：

- 所有基本类型（int、float、boolean等）
- String
- Class
- enum
- Annotation
- 以上类型的数组

如果你使用了其他类型，编译器就会报错。注意，也不允许使用任何包装类型，但是由于自动装箱的存在，这不算是什么限制。注解也可以作为元素的类型。稍后你会看到，注解嵌套是一个非常有用的技巧。



### [默认值限制](https://lingcoder.github.io/OnJava8/#/book/23-Annotations?id=默认值限制)

编译器对于元素的默认值有些过于挑剔。首先，元素不能有不确定的值。也就是说，元素要么有默认值，要么就在使用注解时提供元素的值。

这里有另外一个限制：任何非基本类型的元素， 无论是在源代码声明时还是在***注解接口中定义默认值***时，都***不能使用 null 作为其值***。这个限制使得处理器很难表现一个元素的存在或者缺失的状态，因为在每个注解的声明中，所有的元素都存在，并且具有相应的值。为了绕开这个约束，可以自定义一些特殊的值，比如空字符串或者负数用于表达某个元素不存在。

```java
// annotations/SimulatingNull.java
import java.lang.annotation.*;
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface SimulatingNull {
    int id() default -1;
    String description() default "";
}复制ErrorOK!
```

这是一个在定义注解的习惯用法。



在 `@Target` 注解中指定的每一个 **ElementType** 就是一个约束，它告诉编译器，这个自定义的注解只能用于指定的类型。你可以指定 **enum ElementType** 中的一个值，或者以逗号分割的形式指定多个值。如果想要将注解应用于所有的 **ElementType**，那么可以省去 `@Target` 注解，但是这并不常见。

**ElementType** SourceCode

```java
public enum ElementType {
    /** Class, interface (including annotation type), enum, or record
     * declaration */
    TYPE,

    /** Field declaration (includes enum constants) */
    FIELD,

    /** Method declaration */
    METHOD,

    /** Formal parameter declaration */
    PARAMETER,

    /** Constructor declaration */
    CONSTRUCTOR,

    /** Local variable declaration */
    LOCAL_VARIABLE,

    /** Annotation type declaration */
    ANNOTATION_TYPE,

    /** Package declaration */
    PACKAGE,

    /**
     * Type parameter declaration
     *
     * @since 1.8
     */
    TYPE_PARAMETER,

    /**
     * Use of a type
     *
     * @since 1.8
     */
    TYPE_USE,

    /**
     * Module declaration.
     *
     * @since 9
     */
    MODULE,

    /**
     * {@preview Associated with records, a preview feature of the Java language.
     *
     *           This constant is associated with <i>records</i>, a preview
     *           feature of the Java language. Programs can only use this
     *           constant when preview features are enabled. Preview features
     *           may be removed in a future release, or upgraded to permanent
     *           features of the Java language.}
     *
     * Record component
     *
     * @jls 8.10.3 Record Members
     * @jls 9.7.4 Where Annotations May Appear
     *
     * @since 14
     */
    @jdk.internal.PreviewFeature(feature=jdk.internal.PreviewFeature.Feature.RECORDS,
                                 essentialAPI=true)
    RECORD_COMPONENT;
}
```



在 Java 8，在使用多个注解的时候，你可以重复使用同一个注解。

详细可以参考一下书中对于DBTable设计的注解，以及后续针对主键的注解使用的优化：

​				使用***两个注解***来确定主键及value类型的方案是比较满意的



### [注解不支持继承](https://lingcoder.github.io/OnJava8/#/book/23-Annotations?id=注解不支持继承)

你不能使用 **extends** 关键字来继承 **@interfaces**。这真是一个遗憾，如果可以定义 **@TableColumn** 注解（参考前面的建议），同时嵌套一个 **@SQLType** 类型的注解，将成为一个优雅的设计。按照这种方式，你可以通过继承 **@SQLType** 来创造各种 SQL 类型。例如 **@SQLInteger** 和 **@SQLString**。如果支持继承，就会大大减少打字的工作量并且使得语法更整洁。在 Java 的未来版本中，似乎没有任何关于让注解支持继承的提案，所以在当前情况下，上例中的解决方案可能已经是最佳方案了。



### [实现处理器](https://lingcoder.github.io/OnJava8/#/book/23-Annotations?id=实现处理器)

下面是一个注解处理器的例子，他将读取一个类文件，检查上面的数据库注解，并生成用于创建数据库的 SQL 命令：

```java
// annotations/database/TableCreator.java
// Reflection-based annotation processor
// {java annotations.database.TableCreator
// annotations.database.Member}
package annotations.database;

import java.lang.annotation.Annotation;
import java.lang.reflect.Field;
import java.util.ArrayList;
import java.util.List;

public class TableCreator {
    public static void
    main(String[] args) throws Exception {
        if (args.length < 1) {
            System.out.println(
                    "arguments: annotated classes");
            System.exit(0);
        }
        for (String className : args) {
            Class<?> cl = Class.forName(className);
            DBTable dbTable = cl.getAnnotation(DBTable.class);
            if (dbTable == null) {
                System.out.println(
                        "No DBTable annotations in class " +
                                className);
                continue;
            }
            String tableName = dbTable.name();
            // If the name is empty, use the Class name:
            if (tableName.length() < 1)
                tableName = cl.getName().toUpperCase();
            List<String> columnDefs = new ArrayList<>();
            for (Field field : cl.getDeclaredFields()) {
                String columnName = null;
                Annotation[] anns =
                        field.getDeclaredAnnotations();
                if (anns.length < 1)
                    continue; // Not a db table column
                if (anns[0] instanceof SQLInteger) {
                    SQLInteger sInt = (SQLInteger) anns[0];
                    // Use field name if name not specified
                    if (sInt.name().length() < 1)
                        columnName = field.getName().toUpperCase();
                    else
                        columnName = sInt.name();
                    columnDefs.add(columnName + " INT" +
                            getConstraints(sInt.constraints()));
                }
                if (anns[0] instanceof SQLString) {
                    SQLString sString = (SQLString) anns[0];
                    // Use field name if name not specified.
                    if (sString.name().length() < 1)
                        columnName = field.getName().toUpperCase();
                    else
                        columnName = sString.name();
                    columnDefs.add(columnName + " VARCHAR(" +
                            sString.value() + ")" +
                            getConstraints(sString.constraints()));
                }
                StringBuilder createCommand = new StringBuilder(
                        "CREATE TABLE " + tableName + "(");
                for (String columnDef : columnDefs)
                    createCommand.append(
                            "\n " + columnDef + ",");
                // Remove trailing comma
                String tableCreate = createCommand.substring(
                        0, createCommand.length() - 1) + ");";
                System.out.println("Table Creation SQL for " +
                        className + " is:\n" + tableCreate);
            }
        }
    }

    private static String getConstraints(Constraints con) {
        String constraints = "";
        if (!con.allowNull())
            constraints += " NOT NULL";
        if (con.primaryKey())
            constraints += " PRIMARY KEY";
        if (con.unique())
            constraints += " UNIQUE";
        return constraints;
    }
}
```



### [最简单的处理器](https://lingcoder.github.io/OnJava8/#/book/23-Annotations?id=最简单的处理器)

让我们开始定义我们能想到的最简单的处理器，只是为了编译和测试（用于在编译中输出所有的注解对象）。如下是注解的定义：

```java
// annotations/simplest/Simple.java
// A bare-bones annotation
package annotations.simplest;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
import java.lang.annotation.ElementType;
@Retention(RetentionPolicy.SOURCE)
@Target({ElementType.TYPE, ElementType.METHOD,
        ElementType.CONSTRUCTOR,
        ElementType.ANNOTATION_TYPE,
        ElementType.PACKAGE, ElementType.FIELD,
        ElementType.LOCAL_VARIABLE})
public @interface Simple {
    String value() default "-default-";
}复制ErrorOK!
```

**@Retention** 的参数现在为 **SOURCE**，这意味着注解不会再存留在编译后的代码。这在编译时处理注解是没有必要的，它只是指出，在这里，**javac** 是唯一有机会处理注解的代理。

**@Target** 声明了几乎所有的目标类型（除了 **PACKAGE**） ，同样是为了演示。下面是一个测试示例。

```java
// annotations/simplest/SimpleTest.java
// Test the "Simple" annotation
// {java annotations.simplest.SimpleTest}
package annotations.simplest;
@Simple
public class SimpleTest {
    @Simple
    int i;
    @Simple
    public SimpleTest() {}
    @Simple
    public void foo() {
        System.out.println("SimpleTest.foo()");
    }
    @Simple
    public void bar(String s, int i, float f) {
        System.out.println("SimpleTest.bar()");
    }
    @Simple
    public static void main(String[] args) {
        @Simple
        SimpleTest st = new SimpleTest();
        st.foo();
    }
}复制ErrorOK!
```

输出为：

```java
SimpleTest.foo()
```

在这里我们使用 **@Simple** 注解了所有 **@Target** 声明允许的地方。

**SimpleTest.java** 只需要 **Simple.java** 就可以编译成功。当我们编译的时候什么都没有发生。

**javac** 允许 **@Simple** 注解（只要它存在）在我们创建处理器并将其 hook 到编译器之前，不做任何事情。

如下是一个十分简单的处理器，其所作的事情就是把注解相关的信息打印出来：

```java
// annotations/simplest/SimpleProcessor.java
// A bare-bones annotation processor
package annotations.simplest;
import javax.annotation.processing.*;
import javax.lang.model.SourceVersion;
import javax.lang.model.element.*;
import java.util.*;
@SupportedAnnotationTypes(
        "annotations.simplest.Simple")
@SupportedSourceVersion(SourceVersion.RELEASE_8)
public class SimpleProcessor
        extends AbstractProcessor {
    @Override
    public boolean process(
            Set<? extends TypeElement> annotations,
            RoundEnvironment env) {
        for(TypeElement t : annotations)
            System.out.println(t);
        for(Element el :
                env.getElementsAnnotatedWith(Simple.class))
            display(el);
        return false;
    }
    private void display(Element el) {
        System.out.println("==== " + el + " ====");
        System.out.println(el.getKind() +
                " : " + el.getModifiers() +
                " : " + el.getSimpleName() +
                " : " + el.asType());
        if(el.getKind().equals(ElementKind.CLASS)) {
            TypeElement te = (TypeElement)el;
            System.out.println(te.getQualifiedName());
            System.out.println(te.getSuperclass());
            System.out.println(te.getEnclosedElements());
        }
        if(el.getKind().equals(ElementKind.METHOD)) {
            ExecutableElement ex = (ExecutableElement)el;
            System.out.print(ex.getReturnType() + " ");
            System.out.print(ex.getSimpleName() + "(");
            System.out.println(ex.getParameters() + ")");
        }
    }
}
```

（旧的，失效的）**apt** 版本的处理器需要额外的方法来确定支持哪些注解以及支持的 Java 版本。不过，你现在可以简单的使用 **@SupportedAnnotationTypes** 和 **@SupportedSourceVersion** 注解（这是一个很好的用注解简化代码的示例）。

你唯一需要实现的方法就是 `process()`，这里是所有行为发生的地方。第一个参数告诉你哪个注解是存在的，第二个参数保留了剩余信息。我们所做的事情只是打印了注解（这里只存在一个），可以看 **TypeElement** 文档中的其他行为。通过使用 `process()` 的第二个操作，我们循环所有被 **@Simple** 注解的元素，并且针对每一个元素调用我们的 `display()` 方法。所有 **Element** 展示了自身的基本信息；例如，`getModifiers()` 告诉你它是否为 **public** 和 **static** 。

**Element** 只能执行那些编译器解析的所有基本对象共有的操作，而类和方法之类的东西有额外的信息需要提取。所以（如果你阅读了正确的文档，但是我没有在任何文档中找到——我不得不通过 StackOverflow 寻找线索）你检查它是哪种 **ElementKind**，然后将其向下转换为更具体的元素类型，注入针对 CLASS 的 TypeElement 和针对 METHOD 的ExecutableElement。此时，可以为这些元素调用其他方法。

动态向下转型（在编译期不进行检查）并不像是 Java 的做事方式，这非常不直观这也是为什么我从未想过要这样做事。相反，我花了好几天的时间，试图发现你应该如何访问这些信息，而这些信息至少在某种程度上是用不起作用的恰当方法简单明了的。我还没有遇到任何东西说上面是规范的形式，但在我看来是。

如果只是通过平常的方式来编译 **SimpleTest.java**，你不会得到任何结果。为了得到注解输出，你必须增加一个 **processor** 标志并且连接注解处理器类

```shell
javac -processor annotations.simplest.SimpleProcessor SimpleTest.java
```

现在编译器有了输出

```shell
annotations.simplest.Simple
==== annotations.simplest.SimpleTest ====
CLASS : [public] : SimpleTest : annotations.simplest.SimpleTest
annotations.simplest.SimpleTest
java.lang.Object
i,SimpleTest(),foo(),bar(java.lang.String,int,float),main(java.lang.String[])
==== i ====
FIELD : [] : i : int
==== SimpleTest() ====
CONSTRUCTOR : [public] : <init> : ()void
==== foo() ====
METHOD : [public] : foo : ()void
void foo()
==== bar(java.lang.String,int,float) ====
METHOD : [public] : bar : (java.lang.String,int,float)void
void bar(s,i,f)
==== main(java.lang.String[]) ====
METHOD : [public, static] : main : (java.lang.String[])void
void main(args)复制ErrorOK!
```

这给了你一些可以发现的东西，包括参数名和类型、返回值等。





```shell
javac -encoding UTF-8 -d out/directory  SimpleProcessor.java Simple.java

javac -encoding UTF-8 -cp out/directory -processor com.example.swk.top.testDemo.annotation.SimpleProcessor -d out/processorDeal SimpleTest.java
```



### 3. javac

这里稍微解释一下 `javac` 命令, `IDE` 用多了, 写的时候都忘得差不多了 *(:зゝ∠)*
`javac` 用于启动 java 编译器, 格式为 `javac <options> <source files>`, 其中 `<options>` 的格式为 `-xx xxxx`, 都是配对出现的, 用于指定一些信息.

这里 `<options>` 的位置并没有讲究, 只要在 `javac` 后面就行了, 在两个 `xxx.java` 之间出现也是可以的, 比如: `javac -d out\production\ src\apt\HelloProcessor.java -encoding UTF-8 src\apt\Hello.java` 正常执行.

一些 `<option>`

- `-cp <路径>`
  - 和 `-classpath <路径>` 一样, 用于指定查找用户类文件和注释处理程序的位置
- `-d <目录>`
  - 指定放置生成的类文件的位置
- `-s <目录>`
  - 指定放置生成的源文件的位置
- `-processorpath <路径>`
  - 指定查找注释处理程序的位置
  - 不写的话会使用 `-cp` 的位置
- `-processor <class1>[,<class2>,<class3>...]`
  - 要运行的注释处理程序的名称; 绕过默认的搜索进程



## [本章小结](https://lingcoder.github.io/OnJava8/#/book/23-Annotations?id=本章小结)

注解是 Java 引入的一项非常受欢迎的补充，它提供了一种结构化，并且具有类型检查能力的新途径，从而使得你能够为代码中加入元数据，而且不会导致代码杂乱并难以阅读。使用注解能够帮助我们避免编写累赘的部署描述性文件，以及其他的生成文件。而 Javadoc 中的 @deprecated 被 @Deprecated 注解所替代的事实也说明，与注释性文字相比，注解绝对更适用于描述类相关的信息。

Java 提供了很少的内置注解。这意味着如果你在别处找不到可用的类库，那么就只能自己创建新的注解以及相应的处理器。通过将注解处理器链接到 javac，你可以一步完成编译新生成的文件，简化了构造过程。

API 的提供方和框架将会将注解作为他们工具的一部分。通过 @Unit 系统，我们可以想象，注解会极大的改变我们的 Java 编程体验。





## LomBok学习

https://www.jianshu.com/p/c1ee7e4247bf

**常用的几个注解**：
 @Data ： 注在类上，提供类的get、set、equals、hashCode、canEqual、toString方法
 @AllArgsConstructor ： 注在类上，提供类的全参构造
 @NoArgsConstructor ： 注在类上，提供类的无参构造
 @Setter ： 注在属性上，提供 set 方法
 @Getter ： 注在属性上，提供 get 方法
 @EqualsAndHashCode ：    注在类上，提供对应的 equals 和 hashCode 方法
 @Log4j/@Slf4j   ： 注在类上，提供对应的 Logger 对象，变量名为 log





## JDK动态代理

动态代理会生成一个代理类，代理类为：com.sun.proxy.$Proxy\${num}

***InvocationHandle的实现***

```java
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;

public class MyInvocationHandle implements InvocationHandler {

	private Object target;

	public MyInvocationHandle(Object target) {
		this.target = target;
	}

	@Override
	public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {

		//pre Aop
		System.out.println("pre Aop is here");

		//void method do not need return
		Object obj = method.invoke(this.target, args);

		//ago Aop
		System.out.println("ago Aop is here");

		return obj;
	}
}
```



***Main函数***

```java
import java.lang.reflect.Proxy;

public class AppDemo {

	public static void main(String[] args) {
		BMWCar car1 = new BMWCar();
		MyInvocationHandle handle = new MyInvocationHandle(car1);
		Car car2 = (Car) Proxy.newProxyInstance(
				car1.getClass().getClassLoader(), car1.getClass().getInterfaces(), handle);
		car1.say();
		car2.say();
		System.out.println(car1.getClass() + ":" + car1.hashCode());
		System.out.println(car2.getClass() + ":" + car2.hashCode());
	}
}
```



Proxy.newProxyInstance是jdk动态代理的根本，会生成一个动态代理的类型***class:com.sun.proxy.$Proxy***

```java
@CallerSensitive
public static Object newProxyInstance(ClassLoader loader,
                                      Class<?>[] interfaces,
                                      InvocationHandler h) {
    Objects.requireNonNull(h);

    final Class<?> caller = System.getSecurityManager() == null
                                ? null
                                : Reflection.getCallerClass();

    /*
     * Look up or generate the designated proxy class and its constructor.
     */
    Constructor<?> cons = getProxyConstructor(caller, loader, interfaces);

    return newProxyInstance(caller, cons, h);
}
```

鸣谢：https://www.jianshu.com/p/269afd0a52e6