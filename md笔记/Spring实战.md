# Spring实战学习：

参考：https://blog.csdn.net/weixin_37910453/article/details/90081929	



###Spring项目console输出带颜色

配置VM参数即可

```shell
-Dspring.output.ansi.enabled=ALWAYS
```



## 注解学习

###@Transcational

@Transactional注解里面的各个属性和咱们在上面讲的事务属性里面是一一对应的。用来设置事务的传播行为、隔离规则、回滚规则、事务超时、是否只读。



```tsx
@Target({ElementType.METHOD, ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Inherited
@Documented
public @interface Transactional {

    /**
     * 当在配置文件中有多个 TransactionManager , 可以用该属性指定选择哪个事务管理器。
     */
    @AliasFor("transactionManager")
    String value() default "";

    /**
     * 同上。
     */
    @AliasFor("value")
    String transactionManager() default "";

    /**
     * 事务的传播行为，默认值为 REQUIRED。
     */
    Propagation propagation() default Propagation.REQUIRED;

    /**
     * 事务的隔离规则，默认值采用 DEFAULT。
     */
    Isolation isolation() default Isolation.DEFAULT;

    /**
     * 事务超时时间。
     */
    int timeout() default TransactionDefinition.TIMEOUT_DEFAULT;

    /**
     * 是否只读事务
     */
    boolean readOnly() default false;

    /**
     * 用于指定能够触发事务回滚的异常类型。
     */
    Class<? extends Throwable>[] rollbackFor() default {};

    /**
     * 同上，指定类名。
     */
    String[] rollbackForClassName() default {};

    /**
     * 用于指定不会触发事务回滚的异常类型
     */
    Class<? extends Throwable>[] noRollbackFor() default {};

    /**
     * 同上，指定类名
     */
    String[] noRollbackForClassName() default {};

}
```

### 2.2.1 value、transactionManager属性

​    它们两个是一样的意思。当配置了多个事务管理器时，可以使用该属性指定选择哪个事务管理器。大多数项目只需要一个事务管理器。然而，有些项目为了提高效率、或者有多个完全不同又不相干的数据源，从而使用了多个事务管理器。机智的Spring的Transactional管理已经考虑到了这一点，首先定义多个transactional manager，并为qualifier属性指定不同的值；然后在需要使用@Transactional注解的时候指定TransactionManager的qualifier属性值或者直接使用bean名称。配置和代码使用的例子：



```xml
<tx:annotation-driven/>
 
<bean id="transactionManager1" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="datasource1"></property>
    <qualifier value="datasource1Tx"/>
</bean>
 
<bean id="transactionManager2" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="datasource2"></property>
    <qualifier value="datasource2Tx"/>
</bean>
```



```java
public class TransactionalService {
 
    @Transactional("datasource1Tx")
    public void setSomethingInDatasource1() { ... }
 
    @Transactional("datasource2Tx")
    public void doSomethingInDatasource2() { ... }

}
```

### 2.2.2 propagation属性

​    propagation用于指定事务的传播行为，默认值为 REQUIRED。propagation有七种类型，就是我们在上文中讲到的事务属性传播行为的七种方式，如下所示:

| propagation属性 | 事务属性-传播行为                               | 含义                                                         |
| --------------- | ----------------------------------------------- | ------------------------------------------------------------ |
| REQUIRED        | TransactionDefinition.PROPAGATION_REQUIRED      | 如果当前没有事务，就新建一个事务，如果已经存在一个事务，则加入到这个事务中。这是最常见的选择。 |
| SUPPORTS        | TransactionDefinition.PROPAGATION_SUPPORTS      | 支持当前事务，如果当前没有事务，就以非事务方式执行。         |
| MANDATORY       | TransactionDefinition.PROPAGATION_MANDATORY     | 表示该方法必须在事务中运行，如果当前事务不存在，则会抛出一个异常。 |
| REQUIRES_NEW    | TransactionDefinition.PROPAGATION_REQUIRES_NEW  | 表示当前方法必须运行在它自己的事务中。一个新的事务将被启动。如果存在当前事务，在该方法执行期间，当前事务会被挂起。 |
| NOT_SUPPORTED   | TransactionDefinition.PROPAGATION_NOT_SUPPORTED | 表示该方法不应该运行在事务中。如果当前存在事务，就把当前事务挂起。 |
| NEVER           | TransactionDefinition.PROPAGATION_NEVER         | 表示当前方法不应该运行在事务上下文中。如果当前正有一个事务在运行，则会抛出异常。 |
| NESTED          | TransactionDefinition.PROPAGATION_NESTED        | 如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行与PROPAGATION_REQUIRED类似的操作。 |

### 2.2.3 isolation属性

​    isolation用于指定事务的隔离规则，默认值为DEFAULT。@Transactional的隔离规则和上文事务属性里面的隔离规则也是一一对应的。总共五种隔离规则，如下所示:

| @isolation属性   | 事务属性-隔离规则                                | 含义                                                         | 脏读 | 不可重复读 | 幻读 |
| ---------------- | ------------------------------------------------ | ------------------------------------------------------------ | ---- | ---------- | ---- |
| DEFAULT          | TransactionDefinition.ISOLATION_DEFAULT          | 使用后端数据库默认的隔离级别                                 |      |            |      |
| READ_UNCOMMITTED | TransactionDefinition.ISOLATION_READ_UNCOMMITTED | 允许读取尚未提交的数据变更(最低的隔离级别)                   | 是   | 是         | 是   |
| READ_COMMITTED   | TransactionDefinition.ISOLATION_READ_COMMITTED   | 允许读取并发事务已经提交的数据                               | 否   | 是         | 是   |
| REPEATABLE_READ  | TransactionDefinition.ISOLATION_REPEATABLE_READ  | 对同一字段的多次读取结果都是一致的，除非数据是被本身事务自己所修改 | 否   | 否         | 是   |
| SERIALIZABLE     | TransactionDefinition.ISOLATION_SERIALIZABLE     | 最高的隔离级别，完全服从ACID的隔离级别，也是最慢的事务隔离级别，因为它通常是通过完全锁定事务相关的数据库表来实现的 | 否   | 否         | 否   |

### 2.2.4 timeout

​    timeout用于设置事务的超时属性。

<p style=color:red>ps  :  当使用该属性时，建议数值设置成小于jdbc链接池和默认jdbc超时值<br>
例如：<br>&nbsp&nbsp&nbsp&nbsp
  ## 数据库连接超时时间,默认30秒，即30000<br>&nbsp&nbsp&nbsp&nbsp 
  spring.datasource.hikari.connection-timeout=30000
</p>

### 2.2.5 readOnly

​    readOnly用于设置事务是否只读属性。

### 2.2.6 rollbackFor、rollbackForClassName、noRollbackFor、noRollbackForClassName

​    rollbackFor、rollbackForClassName用于设置那些异常需要回滚；noRollbackFor、noRollbackForClassName用于设置那些异常不需要回滚。他们就是在设置事务的回滚规则。



作者：tuacy
链接：https://www.jianshu.com/p/befc2d73e487
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。



## @Transactional 注解的使用

***@Transactional***在内外方法中的事务控制；

| Propagation属性                     | outMethod()                      | innerMethod                                                  |
| :---------------------------------- | :------------------------------- | :----------------------------------------------------------- |
| Propagation.MANDATORY               | .抛出异常                        | .在outMethod的Transaction中运行                              |
| Propagation.NEVER                   | .不在Transaction中运行           | .抛出异常                                                    |
| Propagation.NOT_SUPPORTED           | .不在Transaction中运行           | .outMethod的Transaction暂停直至innerMethod执行完毕           |
| Propagation.REQUIRED ( **默认值** ) | .新开一个Transaction并在其中运行 | .在outMethod的Transaction中运行                              |
| Propagation.REQUIRES_NEW            | .新开一个Transaction并在其中运行 | .outMethod的Transaction暂停直至innerMethod中新开的Transaction执行完毕 |
| Propagation.SUPPORTS                | .不在Transaction中运行           | .在outMethod的Transaction中运行                              |