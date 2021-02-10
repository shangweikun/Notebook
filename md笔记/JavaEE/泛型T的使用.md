# 泛型T《OnJava8》

***demo*** : /Users/swk/study/testDemo

***原文档***：https://lingcoder.github.io/OnJava8/#/book/20-Generics



###泛型接口

```java
public interface Supplier<T> {
  
}
```



参数化的 `Supplier` 接口确保 `get()` 返回值是参数的类型。`CoffeeSupplier` 同时还实现了 `Iterable` 接口，所以能用于 *for-in* 语句。不过，它还需要知道何时终止循环，这正是第二个构造函数的作用。

下面是另一个实现 `Supplier<T>` 接口的例子，它负责生成 Fibonacci 数列：

```java
// generics/Fibonacci.java
// Generate a Fibonacci sequence
import java.util.function.*;
import java.util.stream.*;

public class Fibonacci implements Supplier<Integer> {
    private int count = 0;
    @Override
    public Integer get() { return fib(count++); }

    private int fib(int n) {
        if(n < 2) return 1;
        return fib(n-2) + fib(n-1);
    }

    public static void main(String[] args) {
        Stream.generate(new Fibonacci())
              .limit(18)
              .map(n -> n + " ")
              .forEach(System.out::print);
    }
}
```



### 泛型方法

作为准则，请“尽可能”使用泛型方法。通常将单个方法泛型化要比将整个类泛型化更清晰易懂。

如果方法是 **static** 的，则无法访问该类的泛型类型参数，因此，如果使用了泛型类型参数，则它必须是泛型方法。

要定义泛型方法，请将泛型参数列表放置在返回值之前，如下所示：

```java
// generics/GenericMethods.java

public class GenericMethods {
    public <T> void f(T x) {
        System.out.println(x.getClass().getName());
    }

    public static void main(String[] args) {
        GenericMethods gm = new GenericMethods();
        gm.f("");
        gm.f(1);
        gm.f(1.0);
        gm.f(1.0F);
        gm.f('c');
        gm.f(gm);
    }
}
/* Output:
java.lang.String
java.lang.Integer
java.lang.Double
java.lang.Float
java.lang.Character
GenericMethods
*/

```

使用泛型方法时，通常不需要指定参数类型，因为编译器会找出这些类型。 这称为 *类型参数推断*。因此，对 `f()` 的调用看起来像普通的方法调用，并且 `f()` 看起来像被重载了无数次一样。它甚至会接受 **GenericMethods** 类型的参数。



#### 一个泛型的Suppier

这是一个为任意具有无参构造方法的类生成 **Supplier** 的类。为了减少键入，它还包括一个用于生成 **BasicSupplier** 的泛型方法：

```java
// onjava/BasicSupplier.java
// Supplier from a class with a no-arg constructor
package onjava;

import java.util.function.Supplier;

public class BasicSupplier<T> implements Supplier<T> {
    private Class<T> type;

    public BasicSupplier(Class<T> type) {
        this.type = type;
    }

    @Override
    public T get() {
        try {
            // Assumes type is a public class:
            return type.newInstance();
        } catch (InstantiationException |
                IllegalAccessException e) {
            throw new RuntimeException(e);
        }
    }

    // Produce a default Supplier from a type token:
    public static <T> Supplier<T> create(Class<T> type) {
        return new BasicSupplier<>(type);
    }
}
```

此类提供了产生以下对象的基本实现：

1. 是 **public** 的。 因为 **BasicSupplier** 在单独的包中，所以相关的类必须具有 **public** 权限，而不仅仅是包级访问权限。
2. 具有无参构造方法。要创建一个这样的 **BasicSupplier** 对象，请调用 `create()` 方法，并将要生成类型的类型令牌传递给它。通用的 `create()` 方法提供了 `BasicSupplier.create(MyType.class)` 这种较简洁的语法来代替较笨拙的 `new BasicSupplier <MyType>(MyType.class)`。



#### 泛型方法的常用方式

```java
static <T> void getClass(@NotNull T t){
		System.out.println(t.getClass().getName());
	}
```





### 泛型擦除

```java
public class Manipulator2<T extends HasF> {
    private T obj;

    Manipulator2(T x) {
        obj = x;
    }

    public void manipulate() {
        obj.f();
    }
}
```

边界 `<T extends HasF>` 声明 T 必须是 HasF 类型或其子类。如果情况确实如此，就可以安全地在 **obj** 上调用 `f()` 方法。

我们说泛型类型参数会擦除到它的第一个边界（可能有多个边界，稍后你将看到）。我们还提到了类型参数的擦除。编译器实际上会把类型参数替换为它的擦除，就像上面的示例，**T** 擦除到了 **HasF**，就像在类的声明中用 **HasF** 替换了 **T** 一样。



***你必须查看所有的代码，从而确定代码是否复杂到必须使用泛型的程度。***



####擦除的代价

***擦除的代价是显著的。***泛型不能用于显式地引用运行时类型的操作中，例如转型、**instanceof** 操作和 **new** 表达式。因为所有关于参数的类型信息都丢失了，当你在编写泛型代码时，必须时刻提醒自己，你只是看起来拥有有关参数的类型信息而已。



擦除移除了方法体中的类型信息，所以在运行时的问题就是***\*边界\****：即对象进入和离开方法的地点。这些正是编译器在编译期执行类型检查并插入转型代码的地点。



### 补偿擦除

有时，我们可以对这些问题进行编程，但是有时必须通过引入类型标签来补偿擦除。这意味着为所需的类型显式传递一个 **Class** 对象，以在类型表达式中使用它。

例如，由于擦除了类型信息，因此在上一个程序中尝试使用 **instanceof** 将会失败。类型标签可以使用动态 `isInstance()` ：	

```java
// generics/ClassTypeCapture.java

class Building {
}

class House extends Building {
}

public class ClassTypeCapture<T> {
    Class<T> kind;

    public ClassTypeCapture(Class<T> kind) {
        this.kind = kind;
    }

    public boolean f(Object arg) {
        return kind.isInstance(arg);
    }

    public static void main(String[] args) {
        ClassTypeCapture<Building> ctt1 =
                new ClassTypeCapture<>(Building.class);
        System.out.println(ctt1.f(new Building()));
        System.out.println(ctt1.f(new House()));
        ClassTypeCapture<House> ctt2 =
                new ClassTypeCapture<>(House.class);
        System.out.println(ctt2.f(new Building()));
        System.out.println(ctt2.f(new House()));
    }
}
/* Output:
true
true
false
true
*/
```



####创建类型的实例

试图在 **Erased.java** 中 `new T()` 是行不通的，部分原因是由于擦除，部分原因是编译器无法验证 **T** 是否具有默认（无参）构造函数。但是在 C++ 中，此操作自然，直接且安全（在编译时检查）：

```c++
// generics/InstantiateGenericType.cpp
// C++, not Java!

template<class T> class Foo {
  T x; // Create a field of type T
  T* y; // Pointer to T
public:
  // Initialize the pointer:
  Foo() { y = new T(); }
};

class Bar {};

int main() {
  Foo<Bar> fb;
  Foo<int> fi; // ... and it works with primitives
}
```



Java 中的解决方案是传入一个工厂对象，并使用该对象创建新实例。方便的工厂对象只是 **Class** 对象，因此，如果使用类型标记，则可以使用 `newInstance()` 创建该类型的新对象：

```java
// generics/InstantiateGenericType.java

import java.util.function.Supplier;

class ClassAsFactory<T> implements Supplier<T> {
    Class<T> kind;

    ClassAsFactory(Class<T> kind) {
        this.kind = kind;
    }

    @Override
    public T get() {
        try {
            return kind.newInstance();
        } catch (InstantiationException |
                IllegalAccessException e) {
            throw new RuntimeException(e);
        }
    }
}

class Employee {
    @Override
    public String toString() {
        return "Employee";
    }
}

public class InstantiateGenericType {
    public static void main(String[] args) {
        ClassAsFactory<Employee> fe =
                new ClassAsFactory<>(Employee.class);
        System.out.println(fe.get());
        ClassAsFactory<Integer> fi =
                new ClassAsFactory<>(Integer.class);
        try {
            System.out.println(fi.get());
        } catch (Exception e) {
            System.out.println(e.getMessage());
        }
    }
}
/* Output:
Employee
java.lang.InstantiationException: java.lang.Integer
*/
```



这样可以编译，但对于 `ClassAsFactory<Integer>` 会失败，这是因为 **Integer** 没有无参构造函数。由于错误不是在编译时捕获的，因此语言创建者不赞成这种方法。他们建议使用显式工厂（**Supplier**）并约束类型，以便只有实现该工厂的类可以这样创建对象。这是创建工厂的两种不同方法：

```java
// generics/FactoryConstraint.java

import onjava.Suppliers;

import java.util.ArrayList;
import java.util.List;
import java.util.function.Supplier;

class IntegerFactory implements Supplier<Integer> {
    private int i = 0;

    @Override
    public Integer get() {
        return ++i;
    }
}

class Widget {
    private int id;

    Widget(int n) {
        id = n;
    }

    @Override
    public String toString() {
        return "Widget " + id;
    }

    public static
    class Factory implements Supplier<Widget> {
        private int i = 0;

        @Override
        public Widget get() {
            return new Widget(++i);
        }
    }
}

class Fudge {
    private static int count = 1;
    private int n = count++;

    @Override
    public String toString() {
        return "Fudge " + n;
    }
}

class Foo2<T> {
    private List<T> x = new ArrayList<>();

    Foo2(Supplier<T> factory) {
        Suppliers.fill(x, factory, 5);
    }

    @Override
    public String toString() {
        return x.toString();
    }
}

public class FactoryConstraint {
    public static void main(String[] args) {
        System.out.println(
                new Foo2<>(new IntegerFactory()));
        System.out.println(
                new Foo2<>(new Widget.Factory()));
        System.out.println(
                new Foo2<>(Fudge::new));
    }
}
/* Output:
[1, 2, 3, 4, 5]
[Widget 1, Widget 2, Widget 3, Widget 4, Widget 5]
[Fudge 1, Fudge 2, Fudge 3, Fudge 4, Fudge 5]
*/
```

**IntegerFactory** 本身就是通过实现 `Supplier<Integer>` 的工厂。 **Widget** 包含一个内部类，它是一个工厂。还要注意，**Fudge** 并没有做任何类似于工厂的操作，并且传递 `Fudge::new` 仍然会产生工厂行为，因为编译器将对函数方法 `::new` 的调用转换为对 `get()` 的调用。



另一种方法是***\*模板方法设计模式\****。在以下示例中，`create()` 是模板方法，在子类中被重写以生成该类型的对象：

```java
// generics/CreatorGeneric.java

abstract class GenericWithCreate<T> {
    final T element;

    GenericWithCreate() {
        element = create();
    }

    abstract T create();
}

class X {
}

class XCreator extends GenericWithCreate<X> {
    @Override
    X create() {
        return new X();
    }

    void f() {
        System.out.println(
                element.getClass().getSimpleName());
    }
}

public class CreatorGeneric {
    public static void main(String[] args) {
        XCreator xc = new XCreator();
        xc.f();
    }
}
/* Output:
X
*/
```

**GenericWithCreate** 包含 `element` 字段，并通过无参构造函数强制其初始化，该构造函数又调用抽象的 `create()` 方法。这种创建方式可以在子类中定义，同时建立 **T** 的类型。



### 边界问题

由于擦除会删除类型信息，因此唯一可用于无限制泛型参数的方法是那些 **Object** 可用的方法。但是，如果将该参数限制为某类型的子集，则可以调用该子集中的方法。为了应用约束，Java 泛型使用了 `extends` 关键字。





#### 无界通配符<?>

* 有很多情况都和你在这里看到的情况类似，即编译器很少关心使用的是原生类型还是 `<?>` 。

* 在这些情况中，`<?>` 可以被认为是一种装饰，但是它仍旧是很有价值的，因为，实际上它是在声明：“我是想用 Java 的泛型来编写这段代码，我在这里并不是要用原生类型，但是在当前这种情况下，泛型参数可以持有任何类型。” 第二个示例展示了无界通配符的一个重要应用。
* 当你在处理多个泛型参数时，有时允许一个参数可以是任何类型，同时为其他参数确定某种特定类型的这种能力会显得很重要：

 ```java
// generics/UnboundedWildcards2.java
import java.util.*;

public class UnboundedWildcards2 {
    static Map map1;
    static Map<?,?> map2;
    static Map<String,?> map3;

    static void assign1(Map map) { 
        map1 = map; 
    }

    static void assign2(Map<?,?> map) { 
        map2 = map; 
    }

    static void assign3(Map<String,?> map) { 
        map3 = map; 
    }

    public static void main(String[] args) {
        assign1(new HashMap());
        assign2(new HashMap());
        //- assign3(new HashMap());
        // warning: [unchecked] unchecked method invocation:
        // method assign3 in class UnboundedWildcards2
        // is applied to given types
        //     assign3(new HashMap());
        //            ^
        //   required: Map<String,?>
        //   found: HashMap
        // warning: [unchecked] unchecked conversion
        //     assign3(new HashMap());
        //             ^
        //   required: Map<String,?>
        //   found:    HashMap
        // 2 warnings
        assign1(new HashMap<>());
        assign2(new HashMap<>());
        assign3(new HashMap<>());
    }
}
 ```





###问题

* 自动装箱不适用于数组，因此我们必须创建 `FillArray.fill()` 的重载版本，或创建产生 **Wrapped** 输出的生成器。 **FillArray** 仅比 `java.util.Arrays.setAll()` 有用一点，因为它返回填充的数组。

* 一个类不能实现同一个泛型接口的两种变体，由于擦除的原因，这两个变体会成为相同的接口。下面是产生这种冲突的情况：

```java
// generics/MultipleInterfaceVariants.java
// {WillNotCompile}
package generics;

interface Payable<T> {}

class Employee implements Payable<Employee> {}

//can not compile
class Hourly extends Employee implements Payable<Hourly> {}

```

**Hourly** 不能编译，因为擦除会将 `Payable<Employe>` 和 `Payable<Hourly>` 简化为相同的类 **Payable**，这样，上面的代码就意味着在重复两次地实现相同的接口。十分有趣的是，如果从 **Payable** 的两种用法中都移除掉泛型参数（就像编译器在擦除阶段所做的那样）这段代码就可以编译。



### [重载](https://lingcoder.github.io/OnJava8/#/book/20-Generics?id=重载)

下面的程序是不能编译的，即使它看起来是合理的：

```java
// generics/UseList.java
// {WillNotCompile}
import java.util.*;

public class UseList<W, T> {
    void f(List<T> v) {}
    void f(List<W> v) {}
}
```

因为擦除，所以重载方法产生了相同的类型签名。

因而，当擦除后的参数不能产生唯一的参数列表时，你必须提供不同的方法名：

```java
// generics/UseList2.java

import java.util.*;

public class UseList2<W, T> {
    void f1(List<T> v) {}
    void f2(List<W> v) {}
}
```



### [古怪的循环泛型](https://lingcoder.github.io/OnJava8/#/book/20-Generics?id=古怪的循环泛型)

为了理解自限定类型的含义，我们从这个惯用法的一个简单版本入手，它没有自限定的边界。

不能直接继承一个泛型参数，但是，可以继承在其自己的定义中使用这个泛型参数的类。也就是说，可以声明：

```java
// generics/CuriouslyRecurringGeneric.java

class GenericType<T> {}

public class CuriouslyRecurringGeneric
  extends GenericType<CuriouslyRecurringGeneric> {}复制ErrorOK!
```

这可以按照 Jim Coplien 在 C++ 中的*古怪的循环模版模式*的命名方式，称为古怪的循环泛型（CRG）。“古怪的循环”是指类相当古怪地出现在它自己的基类中这一事实。 为了理解其含义，努力大声说：“我在创建一个新类，它继承自一个泛型类型，这个泛型类型接受我的类的名字作为其参数。”当给出导出类的名字时，这个泛型基类能够实现什么呢？好吧，Java 中的泛型关乎参数和返回类型，因此它能够产生使用导出类作为其参数和返回类型的基类。它还能将导出类型用作其域类型，尽管这些将被擦除为 **Object** 的类型。下面是表示了这种情况的一个泛型类：

```java
// generics/BasicHolder.java

public class BasicHolder<T> {
    T element;
    void set(T arg) { element = arg; }
    T get() { return element; }
    void f() {
        System.out.println(element.getClass().getSimpleName());
    }
}复制ErrorOK!
```

这是一个普通的泛型类型，它的一些方法将接受和产生具有其参数类型的对象，还有一个方法在其存储的域上执行操作（尽管只是在这个域上执行 **Object** 操作）。 我们可以在一个古怪的循环泛型中使用 **BasicHolder**：

```java
// generics/CRGWithBasicHolder.java

class Subtype extends BasicHolder<Subtype> {}

public class CRGWithBasicHolder {
    public static void main(String[] args) {
        Subtype st1 = new Subtype(), st2 = new Subtype();
        st1.set(st2);
        Subtype st3 = st1.get();
        st1.f();
    }
}
/* Output:
Subtype
*/
```

注意，这里有些东西很重要：新类 **Subtype** 接受的参数和返回的值具有 **Subtype** 类型而不仅仅是基类 **BasicHolder** 类型。这就是 CRG 的本质：基类用导出类替代其参数。这意味着泛型基类变成了一种其所有导出类的公共功能的模版，但是这些功能对于其所有参数和返回值，将使用导出类型。也就是说，在所产生的类中将使用确切类型而不是基类型。因此，在**Subtype** 中，传递给 `set()` 的参数和从 `get()` 返回的类型都是确切的 **Subtype**。



#### 自循环

```java
// generics/SelfBounding.java

class SelfBounded<T extends SelfBounded<T>> {
    T element;
    SelfBounded<T> set(T arg) {
        element = arg;
        return this;
    }
    T get() { return element; }
}

class A extends SelfBounded<A> {}
class B extends SelfBounded<A> {} // Also OK

class C extends SelfBounded<C> {
    C setAndGet(C arg) { 
        set(arg); 
        return get();
    }
}

class D {}
// Can't do this:
// class E extends SelfBounded<D> {}
// Compile error:
//   Type parameter D is not within its bound

// Alas, you can do this, so you cannot force the idiom:
class F extends SelfBounded {}

public class SelfBounding {
    public static void main(String[] args) {
        A a = new A();
        a.set(new A());
        a = a.set(new A()).get();
        a = a.get();
        C c = new C();
        c = c.setAndGet(new C());
    }
}
```



在非泛型代码中，参数类型不能随子类型发生变化：

```java
// generics/OrdinaryArguments.java

class OrdinarySetter {
    void set(Base base) {
        System.out.println("OrdinarySetter.set(Base)");
    }
}

class DerivedSetter extends OrdinarySetter {
    void set(Derived derived) {
        System.out.println("DerivedSetter.set(Derived)");
    }
}

public class OrdinaryArguments {
    public static void main(String[] args) {
        Base base = new Base();
        Derived derived = new Derived();
        DerivedSetter ds = new DerivedSetter();
        ds.set(derived);
        // Compiles--overloaded, not overridden!:
        ds.set(base);
    }
}
/* Output:
DerivedSetter.set(Derived)
OrdinarySetter.set(Base)
*/复制ErrorOK!
```

`set(derived)` 和 `set(base)` 都是合法的，因此 `DerivedSetter.set()` 没有覆盖 `OrdinarySetter.set()` ，而是***\*重载\****了这个方法。从输出中可以看到，在 **DerivedSetter** 中有两个方法，因此基类版本仍旧是可用的，因此可以证明它被重载过。 但是，在使用自限定类型时，在导出类中只有一个方法，并且这个方法接受导出类型而不是基类型为参数：

```java
// generics/SelfBoundingAndCovariantArguments.java

interface SelfBoundSetter<T extends SelfBoundSetter<T>> {
    void set(T arg);
}

interface Setter extends SelfBoundSetter<Setter> {}

public class SelfBoundingAndCovariantArguments {
    void
    testA(Setter s1, Setter s2, SelfBoundSetter sbs) {
        s1.set(s2);
        //- s1.set(sbs);
        // error: method set in interface SelfBoundSetter<T>
        // cannot be applied to given types;
        //     s1.set(sbs);
        //       ^
        //   required: Setter
        //   found: SelfBoundSetter
        //   reason: argument mismatch;
        // SelfBoundSetter cannot be converted to Setter
        //   where T is a type-variable:
        //     T extends SelfBoundSetter<T> declared in
        //     interface SelfBoundSetter
        // 1 error
    }
}
```



###[泛型异常](https://lingcoder.github.io/OnJava8/#/book/20-Generics?id=泛型异常)

由于擦除的原因，**catch** 语句不能捕获泛型类型的异常，因为在编译期和运行时都必须知道异常的确切类型。泛型类也不能直接或间接继承自 **Throwable**（这将进一步阻止你去定义不能捕获的泛型异常）。 但是，类型参数可能会在一个方法的 **throws** 子句中用到。这使得你可以编写随检查型异常类型变化的泛型代码：

```java
// generics/ThrowGenericException.java

import java.util.*;

interface Processor<T, E extends Exception> {
    void process(List<T> resultCollector) throws E;
}

class ProcessRunner<T, E extends Exception>
extends ArrayList<Processor<T, E>> {
    List<T> processAll() throws E {
        List<T> resultCollector = new ArrayList<>();
        for(Processor<T, E> processor : this)
            processor.process(resultCollector);
        return resultCollector;
    }
}

class Failure1 extends Exception {}

class Processor1
implements Processor<String, Failure1> {
    static int count = 3;
    @Override
    public void process(List<String> resultCollector)
    throws Failure1 {
        if(count-- > 1)
            resultCollector.add("Hep!");
        else
            resultCollector.add("Ho!");
        if(count < 0)
            throw new Failure1();
    }
}

class Failure2 extends Exception {}

class Processor2
implements Processor<Integer, Failure2> {
    static int count = 2;
    @Override
    public void process(List<Integer> resultCollector)
    throws Failure2 {
        if(count-- == 0)
            resultCollector.add(47);
        else {
            resultCollector.add(11);
        }
        if(count < 0)
            throw new Failure2();
    }
}

public class ThrowGenericException {
    public static void main(String[] args) {
        ProcessRunner<String, Failure1> runner =
            new ProcessRunner<>();
        for(int i = 0; i < 3; i++)
            runner.add(new Processor1());
        try {
            System.out.println(runner.processAll());
        } catch(Failure1 e) {
            System.out.println(e);
        }

        ProcessRunner<Integer, Failure2> runner2 =
            new ProcessRunner<>();
        for(int i = 0; i < 3; i++)
            runner2.add(new Processor2());
        try {
            System.out.println(runner2.processAll());
        } catch(Failure2 e) {
            System.out.println(e);
        }
    }
}
/* Output:
[Hep!, Hep!, Ho!]
Failure2
*/复制ErrorOK!
```

**Processor** 执行 `process()` 方法，并且可能会抛出具有类型 **E** 的异常。`process()` 的结果存储在 `List<T>resultCollector` 中（这被称为*收集参数*）。**ProcessRunner** 有一个 `processAll()` 方法，它会在所持有的每个 **Process** 对象执行，并返回 **resultCollector** 。 如果不能参数化所抛出的异常，那么由于检查型异常的缘故，将不能编写出这种泛化的代码。





###[潜在类型机制](https://lingcoder.github.io/OnJava8/#/book/20-Generics?id=潜在类型机制)

Java 泛型看起来是向这一方向迈进了一步。当你在编写或使用只是持有对象的泛型时，这些代码将可以工作于任何类型（除了基本类型，尽管正如你所见到的，自动装箱机制可以克服这一点）。或者，换个角度讲，“持有器”泛型能够声明：“我不关心你是什么类型”。如果代码不关心它将要作用的类型，那么这种代码就可以真正地应用于任何地方，并因此而相当泛化。

还是正如你所见到的，当要在泛型类型上执行操作（即调用 **Object** 方法之外的方法）时，就会产生问题。擦除强制要求指定可能会用到的泛型类型的边界，以安全地调用代码中的泛型对象上的具体方法。这是对“泛化”概念的一种明显的限制，因为必须限制你的泛型类型，使它们继承自特定的类，或者实现特定的接口。在某些情况下，你最终可能会使用普通类或普通接口，因为限定边界的泛型可能会和指定类或接口没有任何区别。

某些编程语言提供的一种解决方案称为*潜在类型机制*或*结构化类型机制*，而更古怪的术语称为*鸭子类型机制*，即“如果它走起来像鸭子，并且叫起来也像鸭子，那么你就可以将它当作鸭子对待。”鸭子类型机制变成了一种相当流行的术语，可能是因为它不像其他的术语那样承载着历史的包袱。

泛型代码典型地只能在泛型类型上调用少量方法，而具有潜在类型机制的语言只要求实现某个方法子集，而不是某个特定类或接口，从而放松了这种限制（并且可以产生更加泛化的代码）。正由于此，潜在类型机制使得你可以横跨类继承结构，调用不属于某个公共接口的方法。因此，实际上一段代码可以声明：“我不关心你是什么类型，只要你可以 `speak()` 和 `sit()` 即可。”由于不要求具体类型，因此代码就可以更加泛化。

***潜在类型机制是一种代码组织和复用机制***。有了它，编写出的代码相对于没有它编写出的代码，能够更容易地复用。代码组织和复用是所有计算机编程的基本手段：编写一次，多次使用，并在一个位置保存代码。因为我并未被要求去命名我的代码要操作于其上的确切接口，所以，有了潜在类型机制，我就可以编写更少的代码，并更容易地将其应用于多个地方。



## [Java8 中的辅助潜在类型](https://lingcoder.github.io/OnJava8/#/book/20-Generics?id=java8-中的辅助潜在类型)

先前声明关于 Java 缺乏对潜在类型的支持在 Java 8 之前是完全正确的。但是，Java 8 中的非绑定方法引用使我们能够产生一种潜在类型的形式，以满足创建一段可工作在不相干类型上的代码。因为 Java 最初并不是如此设计，所以结果可想而知，比其他语言中要尴尬一些。但是，至少现在成为了可能，只是缺乏令人惊艳之处。

我在其他地方从没遇过这种技术，因此我将其称为辅助潜在类型。

我们将重写 **DogsAndRobots.java** 来演示该技术。 为使外观看起来与原始示例尽可能相似，我仅向每个原始类名添加了 **A**：

```java
// generics/DogsAndRobotMethodReferences.java

// "Assisted Latent Typing"
import typeinfo.pets.*;
import java.util.function.*;

class PerformingDogA extends Dog {
    public void speak() { System.out.println("Woof!"); }
    public void sit() { System.out.println("Sitting"); }
    public void reproduce() {}
}

class RobotA {
    public void speak() { System.out.println("Click!"); }
    public void sit() { System.out.println("Clank!"); }
    public void oilChange() {}
}

class CommunicateA {
    public static <P> void perform(P performer,
      Consumer<P> action1, Consumer<P> action2) {
        action1.accept(performer);
        action2.accept(performer);
    }
}

public class DogsAndRobotMethodReferences {
    public static void main(String[] args) {
        CommunicateA.perform(new PerformingDogA(),
          PerformingDogA::speak, PerformingDogA::sit);
        CommunicateA.perform(new RobotA(),
          RobotA::speak, RobotA::sit);
        CommunicateA.perform(new Mime(),
          Mime::walkAgainstTheWind,
          Mime::pushInvisibleWalls);
    }
}
/* Output:
Woof!
Sitting
Click!
Clank!
*/复制ErrorOK!
```

**PerformingDogA** 和 **RobotA** 与 **DogsAndRobots.java** 中的相同，不同之处在于它们不继承通用接口 **Performs** ，因此它们没有通用性。

`CommunicateA.perform()` 在没有约束的 **P** 上生成。 只要可以使用 `Consumer <P>`，它在这里就可以是任何东西，这些 `Consumer<P>` 代表不带参数的 **P** 方法的未绑定方法引用。当您调用 **Consumer** 的 `accept()` 方法时，它将方法引用绑定到执行者对象并调用该方法。 由于 [函数式编程](https://lingcoder.github.io/OnJava8/#/book/13-Functional-Programming) 一章中描述的“魔术”，我们可以将任何符合签名的未绑定方法引用传递给 `CommunicateA.perform()` 。

之所以称其为“辅助”，是因为您必须显式地为 `perform()` 提供要使用的方法引用。 它不能只按名称调用方法。

尽管传递未绑定的方法引用似乎要花很多力气，但潜在类型的最终目标还是可以实现的。 我们创建了一个代码片段 `CommunicateA.perform()` ，该代码可用于任何具有符合签名的方法引用的类型。 请注意，这与我们看到的其他语言中的潜在类型有所不同，因为这些语言不仅需要签名以符合规范，还需要方法名称。 因此，该技术可以说产生了更多的通用代码。

为了证明这一点，我还从 **LatentReflection.java** 中引入了 **Mime**。



### [使用**Suppliers**类的通用方法](https://lingcoder.github.io/OnJava8/#/book/20-Generics?id=使用suppliers类的通用方法)

通过辅助潜在类型，我们可以定义本章其他部分中使用的 **Suppliers** 类。 此类包含使用生成器填充 **Collection** 的工具方法。 泛化这些操作很有意义：

```java
// onjava/Suppliers.java

// A utility to use with Suppliers
package onjava;
import java.util.*;
import java.util.function.*;
import java.util.stream.*;

public class Suppliers {
    // Create a collection and fill it:
    public static <T, C extends Collection<T>> C
      create(Supplier<C> factory, Supplier<T> gen, int n) {
        return Stream.generate(gen)
            .limit(n)
            .collect(factory, C::add, C::addAll);
    }

    // Fill an existing collection:
    public static <T, C extends Collection<T>>
      C fill(C coll, Supplier<T> gen, int n) {
        Stream.generate(gen)
            .limit(n)
            .forEach(coll::add);
        return coll;
    }

    // Use an unbound method reference to
    // produce a more general method:
    public static <H, A> H fill(H holder,
      BiConsumer<H, A> adder, Supplier<A> gen, int n) {
        Stream.generate(gen)
            .limit(n)
            .forEach(a -> adder.accept(holder, a));
        return holder;
    }
}复制ErrorOK!
```

`create()` 为你创建一个新的 **Collection** 子类型，而 `fill()` 的第一个版本将元素放入 **Collection** 的现有子类型中。 请注意，还会返回传入的容器的确切类型，因此不会丢失类型信息。[^6]

前两种方法一般都受约束，只能与 **Collection** 子类型一起使用。`fill()` 的第二个版本适用于任何类型的 **holder** 。 它需要一个附加参数：未绑定方法引用 `adder. fill()` ，使用辅助潜在类型来使其与任何具有添加元素方法的 **holder** 类型一起使用。因为此未绑定方法 **adder** 必须带有一个参数（要添加到 **holder** 的元素），所以 **adder** 必须是 `BiConsumer <H，A>` ，其中 **H** 是要绑定到的 **holder** 对象的类型，而 **A** 是要被添加的绑定元素类型。 对 `accept()` 的调用将使用参数 a 调用对象 **holder** 上的未绑定方法 **holder**。

在一个稍作模拟的测试中对 **Suppliers** 工具程序进行了测试，该仿真还使用了本章前面定义的 **RandomList** ：

```java
// generics/BankTeller.java

// A very simple bank teller simulation
import java.util.*;
import onjava.*;

class Customer {
    private static long counter = 1;
    private final long id = counter++;
    @Override
    public String toString() {
        return "Customer " + id;
    }
}

class Teller {
    private static long counter = 1;
    private final long id = counter++;
    @Override
    public String toString() {
        return "Teller " + id;
    }
}

class Bank {
    private List<BankTeller> tellers =
        new ArrayList<>();
    public void put(BankTeller bt) {
        tellers.add(bt);
    }
}

public class BankTeller {
    public static void serve(Teller t, Customer c) {
        System.out.println(t + " serves " + c);
    }
    public static void main(String[] args) {
        // Demonstrate create():
        RandomList<Teller> tellers =
            Suppliers.create(
            RandomList::new, Teller::new, 4);
        // Demonstrate fill():
        List<Customer> customers = Suppliers.fill(
            new ArrayList<>(), Customer::new, 12);
        customers.forEach(c ->
            serve(tellers.select(), c));
        // Demonstrate assisted latent typing:
        Bank bank = Suppliers.fill(
            new Bank(), Bank::put, BankTeller::new, 3);
        // Can also use second version of fill():
        List<Customer> customers2 = Suppliers.fill(
            new ArrayList<>(),
            List::add, Customer::new, 12);
    }
}
/* Output:
Teller 3 serves Customer 1
Teller 2 serves Customer 2
Teller 3 serves Customer 3
Teller 1 serves Customer 4
Teller 1 serves Customer 5
Teller 3 serves Customer 6
Teller 1 serves Customer 7
Teller 2 serves Customer 8
Teller 3 serves Customer 9
Teller 3 serves Customer 10
Teller 2 serves Customer 11
Teller 4 serves Customer 12
*/复制ErrorOK!
```

可以看到 `create()` 生成一个新的 **Collection** 对象，而 `fill()` 添加到现有 **Collection** 中。第二个版本`fill()` 显示，它不仅与无关的新类型 **Bank** 一起使用，还能与 **List** 一起使用。因此，从技术上讲，`fill()` 的第一个版本在技术上不是必需的，但在使用 **Collection** 时提供了较短的语法。







# 总结*

泛型的使用，在于要控制好边界线的情况；

```markdown
>引用内容《OnJava8》
通过它可以编写出更“泛化”的代码，这些代码对于它们能够作用的类型具有更少的限制，因此单个的代码段可以应用到更多的类型上。
```

由于***\*类型擦除\****的存在，Java使用存在一定的风险。

* 一般使用场景主要在***\*java.util.Collection\****中，

* 但同时要掌握好***\*泛型接口\****，***\*泛型方法\****的使用，以及***\*无界通配符<?>\****

