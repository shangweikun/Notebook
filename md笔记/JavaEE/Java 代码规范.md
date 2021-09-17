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



