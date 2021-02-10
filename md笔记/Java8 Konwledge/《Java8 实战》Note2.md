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

 

