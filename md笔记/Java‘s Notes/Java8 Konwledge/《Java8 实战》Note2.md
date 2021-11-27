接上面👆

## *第六章

### 预定义收集器

* 将元素归约和汇总为一个值
* 元素分组
* 元素分区



### 归约和汇总

利用counting工厂，返回收集器计数

```java
		long count = menu.stream().collect(counting());
		long count2 = menu.stream().count();
```



***Collectors***

summingLong | summingDouble | summingInt 通过collect对数据进行汇总计算

reducing - 归约

把流中的第一个项目作为 ***起点*** ，把*恒等函数*作为一个转换函数。

```java
public static void joiningTest() {
		Dish dish1 = new Dish("dish1", 0);
		Dish dish2 = new Dish("dish2", 0);
		List<Dish> list = new ArrayList<>();
		list.add(dish1);
		list.add(dish2);

		String result = list.stream()
				.map(Dish::getName)
				.collect(joining(", "));

		System.out.println(result);
	}
```

```java
public static void reducingTest() {
		Dish dish1 = new Dish("dish1", 10);
		Dish dish2 = new Dish("dish2", 20);
		Dish dish3 = new Dish("dish2", 30);

		List<Dish> list = new ArrayList<>();
		list.add(dish1);
		list.add(dish2);
		list.add(dish3);

		int result = list.stream()
				.collect(reducing(0, Dish::getCalories, Integer::sum));
		int result1 = list.stream()
				.map(Dish::getCalories).reduce(0, Integer::sum);

		System.out.println(result == result1);
	}
```



IntStream可以避免 ***自动拆箱*** 操作



### 分组

grouping - 分组

```java
public static void groupingTest() {
		Dish dish1 = new Dish("dish1", 10);
		Dish dish2 = new Dish("dish2", 20);

		Dish[] dishes = {dish1, dish2};

		Map<CaloricLevel, List<Dish>> result = Arrays.stream(dishes)
				.collect(
						groupingBy(ts ->
								{
									if (ts.getCalories() > 15) return CaloricLevel.FAT;
									return CaloricLevel.NORMAL;
								}
						));
		System.out.println(result.toString());
	}
```

#### 多级分组

多次使用groupingBy语句，可以很好的实现对应的逻辑；



###partitioningBy

多次使用partitioningBy进行分区，一个返回true和false的分组



### Collector接口

1. supplier 新增结果集容器；
2. accumulator 累加结果集方法；
3. finisher 最终转换方法；
4. combiner 合并结果集方法；
5. characteristic 声明收集器规则；



### 开发自己的 - 收集器

问题：构建 - 入参不同与结果参数的收集器

解决：通过自己定义收集器，修改finisher实现

```java
public static Collector<Object, ArrayList<Object>, ArrayList<AExtend>> collector = new Collector<Object, ArrayList<Object>, ArrayList<AExtend>>() {
		@Override
		public Supplier<ArrayList<Object>> supplier() {
			return ArrayList::new;
		}

		@Override
		public BiConsumer<ArrayList<Object>, Object> accumulator() {
			return List::add;
		}

		@Override
		public BinaryOperator<ArrayList<Object>> combiner() {
			return (list1, list2) -> {
				list1.addAll(list2);
				return list1;
			};
		}

		@Override
		public Function<ArrayList<Object>, ArrayList<AExtend>> finisher() {
			return (list1) -> {
				ArrayList<AExtend> list2 = new ArrayList<>();
				for (Object o : list1) {
					A tmp = (A) o;
					AExtend result = new AExtend();
					result.a1 = tmp.a1;
					result.a2 = String.valueOf(count++);
					list2.add(result);
				}
				return (ArrayList<AExtend>) list2;
			};
		}

		//IDENTITY_FINISH 表示完成器返回一个恒等值，会跳过finisher的方法执行
		@Override
		public Set<Characteristics> characteristics() {
			return Collections.unmodifiableSet(EnumSet.of(
					CONCURRENT
			));
		}
	};
```

 





##*第七章

###并行流

​	并行流本质使用的多线程是使用的ForkJoinPool的所提供的线程池

PS：一旦涉及并行流操作，一定要保证操作的原子性；

错误示例：

```java
package com.example.SpringWeb.java8demo.capter7;

import java.util.stream.LongStream;

public class Demo {

	/**
	 * Demo有一个静态成员，Accumulator的静态内部类，在堆上分配的
	 */
	static class Accumulator {
		public long total = 0;

		public void add(long value) {
			total += value;
		}
	}

	public static void main(String[] args) {
		Accumulator accumulator = new Demo.Accumulator();
		LongStream.rangeClosed(0L, 100000L).parallel().forEach(accumulator::add);
		System.out.println(accumulator.total);
	}
}

```

并行流使用要考虑到很多：注重实际中的 ***测量***，***装箱***



本质

### 分治/合并框架

本质的使用 ***类*** 包括一下三种

* ForkJoinPool
* ForkJoinTask
* RecursiveTask



内部使用 ***工作窃取*** 算法，来保证任务的平均分配



####并行的Iterrator接口 - Spliterator

* tryAdvance传递Consumer

* trySplit 是关键
* estimatedSize 估计长度
* characteristic Spliterator的类型 返回一个int型





# *第八章

***lambda***表达式应用在设计模式上，可以达到节约代码的效果，同时可以很好的改善设计模式僵化的问题

为了更好的使用***lambda***表达式，需要将复杂的表达式单独抽出来，通过**方法引用**的方式去调用

​	- 这样能方便我们查找堆栈错误





#*第九章

不同类型的兼容：

* 二进制：程序在JRE环境下，不重新编译，不影响其运行（即链接和运行不受影响）
* 源代码：程序在不修改情况下，可以正常编译
* 函数行为：程序在进行同样的输入情况，能得到同样的结果



### 默认方法的使用

默认方法的引入，导致了类似C++的菱形继承问题，Java通过以下三个规则对方法的使用进行规定

* 类或者父类中显式声明的方法，其优先级高于一切默认方法

```java
public interface A {
  default void hello(){
    System.out.print("hello A");
  }
}

public interface C extends A{
  void hello();
}
```

以上例子中，当有实现类D实现C时，必须要显示的添加hello方法（即A的默认方法其优先级低于C）



* 如果第一条无法判断，方法签名没有区别，那么选择提供最具体实现的默认方法的借口



* 最后仍无法解决冲突，只能在你的类中覆盖该默认方法，显示地指定你的类中需要使用的方法，使用 ***super*** 关键字

```java
public interface A {
  default void hello(){
    System.out.print("hello A");
  }
}

public interface B {
  default void hello(){
    System.out.print("hello B");
  }
}

public class D implements A,B{
  void hello(){
    B.super.hello();
  }
}
```



#*第十章

* Optional替代null参数

为了解决 空指针 问题，以及解决代码优雅问题，引入***Optional***

```java
package com.example.SpringWeb.java8demo.capter10;

import lombok.Getter;
import lombok.Setter;

import java.util.Optional;

public class Demo {

	public static class Person {
		@Getter
		@Setter
		String personName;

		@Getter
		@Setter
		Car car;

		public Optional<Car> getCarAsOptional() {
			return Optional.of(this.car);
		}
	}

	public static class Car {
		@Getter
		@Setter
		String carName;

		@Getter
		@Setter
		Insurance insurance;

		public Optional<Insurance> getInsuranceAsOptional() {
			return Optional.of(this.insurance);
		}


	}

	public static class Insurance {
		@Getter
		@Setter
		String insName;
	}

	public static void main(String[] args) {
		Person person1 = new Person();
		person1.setPersonName("person1");
		Car car1 = new Car();
		car1.setCarName("car1");
		Insurance insurance1 = new Insurance();
		insurance1.setInsName("insurance1");

		person1.setCar(car1);
		car1.setInsurance(insurance1);

		Optional<Person> optPerson = Optional.of(person1);
		String insName = optPerson
				.flatMap(Person::getCarAsOptional)
				.flatMap(Car::getInsuranceAsOptional)
				.map(Insurance::getInsName)
				.orElse("unknown");
		System.out.println(insName);
	}
}
```



* map
* flatMap
* filter



#*第十一章

***CompletableFuture***

同步转异步，将多个任务并发执行，提高效率；采用ForkJoinPool的池提供的功能；

------

思考：

数字名片中红包使用的池

1. 本质在于往核心上送时增加一个 ***线程池的队列*** 控制，保证在web容器中间件的框架下，增加一个可以由开发人员确定的最大上送核心的TPS（瞬时并发数），其本质仍为同步，并未实现所谓的异步并发功能

2. 其次，任务提交需要和数据库操作放在一个事物中，保证一致性

   （目前是在数据库操作之后，可能存在的问题为：队列已满，未上送核心就已经报错，但红包金额已经扣减）

3. 线程池内管理的线程必须要设置为守护线程

-----



* thenCompose 方法

  当CompletableFuture的第一个方法执行完毕后，将结果作为传入的函数参数去执行调用

前后两个任务在CPU时许上仍是串行，通过同步的方式在进行执行



* thenCombine｜thenCombineAsync





