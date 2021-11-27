# 《Java8 实战》

## *章节2

### 行为参数化

将一个方法，接收多个不同的行为作为***参数***，并在内部使用他们，完成***不同***的行为能力;

可以让代码更好的适应不断变化的需求，减少重复代码的编写，减轻未来的工作量；

行为可以通过参数进行传递，从而达到***传递代码***的目的，以减少重复的代码；



***Don't repeat yourself***

涉及到的设计模式：***策略设计模式***



例如：苹果筛选问题。

行为接口：

```java
package com.example.SpringWeb.java8demo.capter1;

public interface ApplePredicate<T> {
	boolean test(T a);
}
```



```java
package com.example.SpringWeb.java8demo;

import com.example.SpringWeb.java8demo.capter1.Apple;
import com.example.SpringWeb.java8demo.capter1.ApplePredicate;

import java.util.ArrayList;
import java.util.List;

public class Demo {
	public static <T> List filterApple(List<T> inventory, ApplePredicate<T> predicate) {
		inventory.removeIf((T t) -> !predicate.test(t));
		return inventory;
	}

	public static void main(String[] args) {
		Apple a = new Apple(140, "GREEN");
		Apple b = new Apple(160, "Red");
		List<Apple> inventory = new ArrayList<>();
		inventory.add(a);
		inventory.add(b);
    //通过匿名内部类来实现
		filterApple(inventory, new ApplePredicate<Apple>() {
			@Override
			public boolean test(Apple a) {
				return 150 > a.getWeight();
			}
		});
    //通过Lambda表达式实现
		filterApple(inventory, (Apple a1) -> "Red".equals(a1.getColor()));
		System.out.println(inventory.size());
	}
}
```



####Java API接口可以传递的行为

排序，线程，GUI处理

排序：

```java
{
  		List<Apple> inventory = new ArrayList<>();
  		inventory.sort((Apple a1, Apple a2) -> a1.getWeight() - a2.getWeight());
}	
```



##*章节3

###Lambda

函数式接口：***只定义一个抽象方法的接口***



####类型检查

* Lambda的类型是通过使用 ***Lambda的上下文*** 推到出来的

#### 类型推断

* java编译器通过 ***上下文（目标类型）*** 找到适合Lambda表达式的函数式接口



### 方法引用

粗体表示类绑定；斜体表示实例绑定

| Lambda                                      | 方法引用                        |
| ------------------------------------------- | ------------------------------- |
| (args) -> ClassName.staticMethod( args )    | **ClassName**::**staticMethod** |
| (arg0, rest) -> arg0.instanceMethod( rest ) | **ClassName**::*instanceMethod* |
| (args) -> expr.instanceMethod( args )       | *expr*::*instanceMethod*        |



### jdk API 函数式接口

函数式接口统一用 ***@FunctionalInterface*** 接口来进行声明

```java
public interface Predicate<T>
```

```java
public interface Function<T, R>
```

```java
public interface Consumer<T> 
```



### Java8 苹果🍎比较重量问题

从接口一步一步变化到方法引用

```java
package com.example.SpringWeb.java8demo.capter3;

import com.example.SpringWeb.java8demo.capter1.Apple;
import com.example.SpringWeb.java8demo.capter3.lambdaTest.AppleComparator;

import java.util.ArrayList;
import java.util.Comparator;
import java.util.List;

import static java.util.Comparator.comparing;

public class Demo1 {

	public static void main(String[] args) {
		Apple apple1 = new Apple();
		Apple apple2 = new Apple();
		apple1.setWeight(100);
		apple2.setWeight(200);
		List<Apple> apples = new ArrayList<>();

		apples.add(apple2);
		apples.add(apple1);
		/** 第1步 增加比较器类 **/
		AppleComparator comparator = new AppleComparator();
		apples.sort(comparator);

		apples.clear();
		apples.add(apple2);
		apples.add(apple1);
		/** 第2步 匿名内部类 **/
		apples.sort(new Comparator<Apple>() {
			@Override
			public int compare(Apple o1, Apple o2) {
				return o1.getWeight().compareTo(o2.getWeight());
			}
		});

		apples.clear();
		apples.add(apple2);
		apples.add(apple1);
		/** 第3步 使用lambda表达式 **/
		apples.sort((o1,o2)->o1.getWeight().compareTo(o2.getWeight()));
		apples.sort(comparing(o1->o1.getWeight()));

		apples.clear();
		apples.add(apple2);
		apples.add(apple1);
		/** 第4步 方法引用引入 **/
		apples.sort(comparing(Apple::getWeight));

	}
}
```



## *章节4

### 流

####高层次构件：

​	filter，sorted，map，collect等操作是与具体线程模型无关的 ***高层次构件***



###Stream API的特性

声明性：简洁，易读；

可复合：更灵活；

可并行：性能更好；



#### 一系列操作

filter，map，limit，collect

#### filter - 接受Lambd

​				从流中过滤某些数据；

#### map - 接受Lambda

​				将元素转换成其他形式或提取信息；

#### limit - 截断流

#### collect - 将流转换为列表

```java
package com.example.SpringWeb;

import java.util.ArrayList;
import java.util.List;

import static java.util.stream.Collectors.toList;

public class Test0 {
	public static void main(String[] args) {
		List<String> strings = new ArrayList<>();
		strings.add("a");
		strings.add("ab");
		strings.add("abc");
		strings.add("abcd");
		List<String> strings2 = strings.stream()
				.filter(s -> s.equals("target") || s.length() > 2)
				.map(s -> s.substring(1))
				.collect(toList());
		for (String s : strings2
		) {
			System.out.println(s);
		}
	}

}
```



### 流与集合的区别

* 1、流只能被消费一次

  ```java
  Stream<String> stream = strings.stream();
  stream.forEach(System.out::println);
  //java.lang.IllegalStateException
  stream.forEach(System.out::println);
  ```

  

### 流的中间和终端操作

连接流的操作；关闭流的操作；

#### 终端操作

* 1、forEach
* 2、collect



### 构建器模式

http://en.wikipedia.org/wiki/Builder_pattern



## *章节5

### 筛选和切片

筛选操作：filter，distinct

切片操作：limit，skip（跳过前n个数据）



###映射map

对流中的元素应用函数：map

流的拍平操作：flatMap

```java
		String[] arrays = {"Hello", " world"};
		Stream<String> stream3 = Arrays.stream(arrays);
		List<Stream> streams = stream3
				.map(s -> s.split(""))
				.map(Arrays::stream)
				.distinct()
				.collect(toList());
		stream3 = Arrays.stream(arrays);
		List<String> stream4 = stream3
				.map(s -> s.split(""))
				.flatMap(Arrays::stream)
				.distinct()
				.collect(toList());
		streams.forEach(System.out::println);
		stream4.forEach(System.out::println);
```



### 查找和匹配

anyMatch - 任一匹配 

allMatch - 全部匹配 

noneMatch - 全部不匹配 

以上三个均为 ***终端操作，返回boolean***



findAny｜findFirst 返回 ***Opitional***

```java
class Opitional
```

两者的区别在于 ***并行流*** - 请尽量使用findAny



### 归约操作

1、元素求和

```java
	public static int sum(int[] numbers) {
		int result = Arrays.stream(numbers).reduce(0, (a, b) -> a + b);
		return result;
	}
```

2、最大值

```java
	public static int max(int[] numbers) {
		OptionalInt max = Arrays.stream(numbers).reduce(Integer::max);
		return max.getAsInt();
	}
```

3、你到底有多少盘菜？

```java
		Arrays.stream(dishes).map(d->1).reduce(0,(a,b)->a+b);
```



### 数值流问题

***IntStream.rangeClosed(1,100)*** 产生一个数值流｜ ***rangge*** 不包含结束值

实战使用

```java
Stream<int[]> ints = IntStream.range(1, 100)
				.filter(b -> Math.sqrt(a * a + b * b) % 1 == 0)
				.boxed()    //注释掉后异常 原因：IntStream的流map方法只允许返回int类型
				.map(b -> new int[]{a, b, (int) Math.sqrt(a * a + b * b)});
```



### 流无处不在

获取流

***Stream.of*** ｜ ***Stream.empty***

从数组创建流

***Arrays.stream***



#### 文件生成流

java.nio.file.Files

```java
	public static void fileTest() {
		long uniqueWords = 0;
		try (Stream<String> lines
				     = Files.lines(Paths.get("data.txt"), Charset.defaultCharset())) {
			uniqueWords = lines.flatMap(line -> Arrays.stream(line.split("")))
					.distinct()
					.count();
		} catch (IOException e) {
			e.printStackTrace();
		}
		System.out.println(uniqueWords);
	}
```



#### 由函数生成流：创建无限流

斐波那契数列：

```java
	public static void fibonacciSequence(){
		Stream.iterate(new int[]{0,1}, t->new int[]{t[1],t[0]+t[1]})
				.limit(10)
				.map(t->t[0])
				.forEach(System.out::println);
	}
```

斐波那契数列2：

```java
	public static void fibonacciSequence2() {

		IntSupplier fib = new IntSupplier() {
			private int previous = 0;
			private int current = 1;

			@Override
			public int getAsInt() {
				int oldPrevious = this.previous;
				int nextValue = this.previous + this.current;
				this.previous = this.current;
				this.current = nextValue;
				return oldPrevious;
			}
		};
		IntStream.generate(fib).limit(10).forEach(System.out::println);
	}
```

在第二个例子中，会通过调用 ***getAsInt*** 时，会改变对象的 ***状态***



**中间操作方法分类：**

- filter()
- map()
- flatMap()
- distinct()
- sorted()
- peek()
- limit()
- skip()

**终端操作方法分类：**

- forEach()
- forEachOrdered()
- toArray()
- reduce()
- collect()
- min()
- max()
- count()
- anyMatch()
- allMatch()
- noneMatch()
- findFirst()
- findAny()