接上面👆

# PART4



## 函数式编程

使用函数进行编程的方式

* 函数式：函数或者方法不应该抛出任何异常



1. 尽量不要修改传入的参数变量，如果依赖，请构建一个新的返回结果；
2. 递归使用时，请尽量避免使用上一步递归中使用递归的结果；

```java
static long factorialRecursive(long n){
  return n == 1 ? 1 : n * factorialRecursive( n - 1 );
}
```

尽量使用以下 ***尾 - 递***：

```java
static long factorialRecursive(long acc, long n){
	return n ==1 ? acc : factorialRecursive(acc * n, n-1);
}
```

scala和groovy已经实现了该种功能，进行优化递归（《JAVA 8 IN ACTION》出书时，未实现）





## 科里化

用数学定义来思考：

f(x,y) = g(x)(y)

g(x) 为一个新的函数



## 延迟列表

即使用时生成必要数据



## 模式匹配

