## 永久代（PermGen）



###JDK7的时代

Java7及以前版本的Hotspot中方法区位于永久代中。同时，永久代和堆是相互隔离的，但它们使用的物理内存是连续的。

Java7中永久代中存储的部分数据已经开始转移到Java Heap或Native Memory中了，符号引用(Symbols)转移到了Native Memory；字符串常量池(interned strings)转移到了Java Heap；类的静态变量(class statics)转移到了Java Heap。



###JDK8的时代

在Java8中，时代变了，Hotspot取消了永久代。永久代真的成了永久的记忆。永久代的参数-XX:PermSize和-XX：MaxPermSize也随之失效。





## 元空间(Metaspace)

​        在Java8中，元空间(Metaspace)登上舞台，方法区存在于元空间(Metaspace)。同时，元空间不再与堆连续，而且是存在于 ***本地内存*** （Native memory）。



默认情况下元空间是可以无限使用本地内存的，但为了不让它如此膨胀，JVM同样提供了参数来限制它使用的使用。

- -XX:MetaspaceSize，class metadata的初始空间配额，以bytes为单位，达到该值就会触发垃圾收集进行类型卸载，同时GC会对该值进行调整：如果释放了大量的空间，就适当的降低该值；如果释放了很少的空间，那么在不超过MaxMetaspaceSize（如果设置了的话），适当的提高该值。
- -XX：MaxMetaspaceSize，可以为class metadata分配的最大空间。默认是没有限制的。
- -XX：MinMetaspaceFreeRatio,在GC之后，最小的Metaspace剩余空间容量的百分比，减少为class metadata分配空间导致的垃圾收集。
- -XX:MaxMetaspaceFreeRatio,在GC之后，最大的Metaspace剩余空间容量的百分比，减少为class metadata释放空间导致的垃圾收集。

# 引言：永久代为什么被移出HotSpot JVM了？

在 HotSpot JVM 中，永久代中用于存放类和方法的元数据以及常量池，比如`Class`和`Method`。每当一个类初次被加载的时候，它的元数据都会放到永久代中。

永久代是有大小限制的，因此如果加载的类太多，很有可能导致永久代内存溢出，即万恶的 *java.lang.OutOfMemoryError: PermGen* ，为此我们不得不对虚拟机做调优。

那么，Java 8 中 PermGen 为什么被移出 HotSpot JVM 了？我总结了两个主要原因（详见：[JEP 122: Remove the Permanent Generation](http://openjdk.java.net/jeps/122)）：

1. 由于 PermGen 内存经常会溢出，引发恼人的 *java.lang.OutOfMemoryError: PermGen*，因此 JVM 的开发者希望这一块内存可以更灵活地被管理，不要再经常出现这样的 OOM
2. 移除 PermGen 可以促进 HotSpot JVM 与 JRockit VM 的融合，因为 JRockit 没有永久代。

根据上面的各种原因，PermGen 最终被移除，**方法区移至 Metaspace，字符串常量移至 Java Heap**。



# 探秘元空间

由于 Metaspace 的资料比较少，这里主要是依据Oracle官方的Java虚拟机规范及Oracle Blog里的几篇文章来总结的。

首先，Metaspace（元空间）是哪一块区域？官方的解释是：

> In JDK 8, classes metadata is now stored in the **native heap** and this space is called **Metaspace**.

也就是说，JDK 8 开始把类的元数据放到本地堆内存(native heap)中，这一块区域就叫 Metaspace，中文名叫元空间。

## 优点

使用本地内存有什么好处呢？最直接的表现就是OOM问题将不复存在，因为默认的类的元数据分配只受本地内存大小的限制，也就是说本地内存剩余多少，理论上Metaspace就可以有多大（貌似容量还与操作系统的虚拟内存有关？这里不太清楚），这解决了空间不足的问题。不过，让 Metaspace 变得无限大显然是不现实的，因此我们也要限制 Metaspace 的大小：使用 **-XX:MaxMetaspaceSize** 参数来指定 Metaspace 区域的大小。JVM 默认在运行时根据需要动态地设置 **MaxMetaspaceSize** 的大小。

除此之外，它还有以下优点：

- Take advantage of Java Language Specification property : Classes and associated metadata lifetimes match class loader’s
- Linear allocation only
- No individual reclamation (except for RedefineClasses and class loading failure)
- No GC scan or compaction
- No relocation for metaspace objects

## GC

如果Metaspace的空间占用达到了设定的最大值，那么就会触发GC来收集死亡对象和类的加载器。根据JDK 8的特性，G1和CMS都会很好地收集Metaspace区（一般都伴随着Full GC）。

为了减少垃圾回收的频率及时间，控制吞吐量，对Metaspace进行适当的监控和调优是非常有必要的。如果在Metaspace区发生了频繁的Full GC，那么可能表示存在内存泄露或Metaspace区的空间太小了。

## 新增的 JVM 参数

- **-XX:MetaspaceSize** 是分配给类元数据空间（以字节计）的初始大小(Oracle逻辑存储上的初始高水位，*the initial high-water-mark* )，此值为估计值。MetaspaceSize的值设置的过大会延长垃圾回收时间。垃圾回收过后，引起下一次垃圾回收的类元数据空间的大小可能会变大。
- **-XX:MaxMetaspaceSize** 是分配给类元数据空间的最大值，超过此值就会触发Full GC，此值默认没有限制，但应取决于系统内存的大小。JVM会动态地改变此值。
- **-XX:MinMetaspaceFreeRatio** 表示一次GC以后，为了避免增加元数据空间的大小，空闲的类元数据的容量的最小比例，不够就会导致垃圾回收。
- **-XX:MaxMetaspaceFreeRatio** 表示一次GC以后，为了避免增加元数据空间的大小，空闲的类元数据的容量的最大比例，不够就会导致垃圾回收。

## 监控与调优（待补充）

`VisualVM`、`jstat`、`jstack` 可以监测 Metaspace 的动态，后续将更新这里。

鸣谢：

[url]: sczyh30.com/posts/Java/jvm-metaspace/	"元数据空间探秘"





> 1. Does the structure of memory areas depend on the implementation of JVM ?

Absolutely. PermGen or Metaspace are just implementation details of a particular JVM. The following answers are about HotSpot JVM, the reference implementation of Java SE virtual machine.

> 1. What areas does the heap include in java8+ ?

For the above reason it would be more correct to say "in JDK 8" rather than "in Java 8".

The structure of Java Heap depends on the selected GC algorithm. E.g. with Parallel GC and CMS the heap is divided into Old and Young generations, where the latter consists of Eden and two Survivor Spaces.

G1 Heap is divided into the regions of the same size. Epsilon GC heap is a single monolithic area. And so on.

> 1. Where the static methods and variables are stored before java8 and java8+ ?

Methods (both static and non-static) reside in Metaspace in JDK 8 or in PermGen prior to JDK 8. Not sure what you mean by "variables": field values are in Java Heap, the field metadata (names, types, offsets) is in Metaspace.

> 1. Does the MetaSpace store anything except class meta-data info ?

The following items are stored in Metaspace:

- Classes (their internal representation)
- Symbols (names and signatures)
- Primitive arrays (e.g. field metadata is represented as an array of unsigned shorts)
- Methods and their bytecodes
- Method counters
- Constant pools and CP caches
- Annotations

All of these can be considered "class metadata".

参考：

[stackOverFlow]: https://stackoverflow.com/questions/53678418/java-memory-areas-in-java-8	"Java memory areas in java 8"





## super

- 访问父类的构造函数：可以使用 super() 函数访问父类的构造函数，从而委托父类完成一些初始化的工作。
- 访问父类的成员：如果子类重写了父类的某个方法，可以通过使用 super 关键字来引用父类的方法实现。



在调用一个方法时，先从本类中查找看是否有对应的方法，如果没有查找到再到父类中查看，看是否有继承来的方法。否则就要对参数进行转型，转成父类之后看是否有对应的方法。总的来说，方法调用的优先级为：

- this.func(this)
- super.func(this)
- this.func(super)
- super.func(super)





private 方法隐式地被指定为 final，如果在子类中定义的方法和基类中的一个 private 方法签名相同，此时子类的方法不是重写基类方法，而是在子类中定义了一个新的方法。





**2. 静态方法** 

静态方法在类加载的时候就存在了，它不依赖于任何实例。所以静态方法必须有实现，也就是说它不能是抽象方法。

```java
public abstract class A {
    public static void func1(){
    }
    // public abstract static void func2();  // Illegal combination of modifiers: 'abstract' and 'static'
}
```

只能访问所属类的静态字段和静态方法，方法中不能有 this 和 super 关键字。

```java
public class A {

    private static int x;
    private int y;

    public static void func1(){
        int a = x;
        // int b = y;  // Non-static field 'y' cannot be referenced from a static context
        // int b = this.y;     // 'A.this' cannot be referenced from a static context
    }
}
```



非静态内部类依赖于外部类的实例，而静态内部类不需要。

```java
public class OuterClass {

    class InnerClass {
    }

    static class StaticInnerClass {
    }

    public static void main(String[] args) {
        // InnerClass innerClass = new InnerClass(); // 'OuterClass.this' cannot be referenced from a static context
        OuterClass outerClass = new OuterClass();
        InnerClass innerClass = outerClass.new InnerClass();
        StaticInnerClass staticInnerClass = new StaticInnerClass();
    }
}
```

静态内部类不能访问外部类的非静态的变量和方法。