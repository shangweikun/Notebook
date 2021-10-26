# DO、DTO、BO、AO、VO、POJO定义

分层领域模型规约：

- DO（ Data Object）：与数据库表结构一一对应，通过DAO层向上传输数据源对象。
- DTO（ Data Transfer Object）：数据传输对象，Service或Manager向外传输的对象。
- BO（ Business Object）：业务对象。 由Service层输出的封装业务逻辑的对象。
- AO（ Application Object）：应用对象。 在Web层与Service层之间抽象的复用对象模型，极为贴近展示层，复用度不高。
- VO（ View Object）：显示层对象，通常是Web向模板渲染引擎层传输的对象。
- POJO（ Plain Ordinary Java Object）：在本手册中， POJO专指只有setter/getter/toString的简单类，包括DO/DTO/BO/VO等。
- Query：数据查询对象，各层接收上层的查询请求。 注意超过2个参数的查询封装，禁止使用Map类来传输。

领域模型命名规约：

- 数据对象：xxxDO，xxx即为数据表名。
- 数据传输对象：xxxDTO，xxx为业务领域相关的名称。
- 展示对象：xxxVO，xxx一般为网页名称。
- POJO是DO/DTO/BO/VO的统称，禁止命名成xxxPOJO。





# 常量静态类的使用

1. 通常使用 `interface` 来替代 `class` 来存放静态不变对象，代码更简洁， 生成的class文件更小，JVM不要考虑类的附加特性，如方法重载等，因而更为高效
2. 不要使用 ***常量接口模式*** ，本身是接口使用的倒车
3. 不要使用静态导入，`import static ***`，因为`import static`会导致可维护性下降，维护的人看代码时，不是那么清楚或者不那么迅速的知道这个常量位于哪个文件中。建议使用常量的地方直接 "接口.常量名" 的方式使用



**静态常量类**：私有话构造函数 

```java
public final class Constans{
  
    private Constans() {
    }
  
    public static final int AUDIT_STATUS_PASS = 1;
    public static final int AUDIT_STATUS_NOT_PASS = 2;
}
```



鸣谢 : https://www.cnblogs.com/easonjim/p/7871753.html





# 避免if-else的使用

可以通过使用`Dictionary`(或者`Map`)，来尽量避免选择判断条件的使用



鸣谢：https://mp.weixin.qq.com/s/NMDDNZKRJaa0K-k1Jjw8UQ





# parallelStream使用



***parallelStream的并行度***

并行度到底是多少，是由下面的参数来控制的。

`-Djava.util.concurrent.ForkJoinPool.common.parallelism=N`

如果无法获取这个参数，则默认使用 `CPU个数-1` 的并行度。

`parallelism`这个变量是final的，一旦设定，不允许修改。也就是说，上面的参数只会生效一次



***总结***

不同的系统，不同的场景下，可能对于并行度的要求不同；

*   特别是在一些 **I/O密集型 ** 的场景下使用，很容易造成并行转化为串行；

*   而另外一些场景下并行度太大，浪费了CPU上下文切换时间，得不偿失。



***解决方案***

可以通过提供外置的forkjoinpool，也就是改变提交方式，来实现不同类型的任务分离。

代码如下所示，通过显式的代码提交，即可实现任务分离。

```java
ForkJoinPool pool = new ForkJoinPool(30);

finallong begin = System.currentTimeMillis();
try {
    pool.submit(() ->
            numbers.parallelStream().map(k -> {
                try {
                    Thread.sleep(1000);
                    System.out.println((System.currentTimeMillis() - begin) + "ms => " + k + " \t" + Thread.currentThread());
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                return k;
            }).collect(Collectors.toList())).get();
} catch (InterruptedException e) {
    e.printStackTrace();
} catch (ExecutionException e) {
    e.printStackTrace();
}
```

这样，不同的场景，就可以拥有不同的并行度。

这种方式和CountDownLatch有异曲同工之妙，我们需要手动管理资源。



使用了这种方式，代码量增加，已经和`优雅`关系不大了，不仅不优雅，而且丑的要命。

白天鹅变成了丑小鸭，你还会爱它么？



鸣谢： [小姐姐味道](javascript:void(0);) 

https://mp.weixin.qq.com/s?__biz=MzA4MTc4NTUxNQ==&mid=2650522238&idx=1&sn=42589cf94ac288aa29f495517e877937&chksm=8780c5bab0f74cac5c0517f4d7874b335c9bbb81a4fd1b5bdf4446c6048b9fb668eee721fc4f&scene=21#wechat_redirect
