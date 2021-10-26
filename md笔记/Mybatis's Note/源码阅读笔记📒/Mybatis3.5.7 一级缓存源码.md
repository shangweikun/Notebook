### 一级缓存的工作流程

插入一张copy的图片

![一级缓存图片](http://42.193.118.12/images/firstCache.png)

简单说明下整个流程

1.   当用户发起查询时，MyBatis根据当前执行的语句生成`MappedStatement`；
2.   在Local Cache进行查询，如果缓存命中的话，直接返回结果给用户；
3.   如果缓存没有命中的话，查询数据库，结果写入`Local Cache`，最后返回结果给用户。



### 源码分析

我们根据上面的时序图，对与这部分的一级缓存代码进行简阅：



#### 主要类

***SqlSession*** 

主要提供了用户一些与数据库交互的方法，并隐藏了部分功能；默认实现类是 ***DefaultSqlSession***

>The primary Java interface for working with MyBatis. Through this interface you can execute commands, get mappers and manage transactions.
>Author:
>Clinton Begin

![SqlSession](http://42.193.118.12/images/SqlSession.png)

***Executor***

 `SqlSession`最终是通过它来与数据库或者缓存进行数据获取的；

该类有很多实现方式，一级缓存的使用是在最基本的抽象类中 ***BaseExcutor***

![Excutor](http://42.193.118.12/images/Excutor.jpg)



***BaseExcutor***

大家可以看一下下面这张图片中，该类所拥有的一些成员变量及属性

![BaseExecutor 属性及成员变量](http://42.193.118.12/images/BaseExcutorFieldAndProperties.png)

一级缓存的实现，与其中的`localCache`是分不开的，其查询和写入都是在该类中进行的；



***Cache***

>SPI for cache providers.
>One instance of cache will be created for each namespace.
>The cache implementation must have a constructor that receives the cache id as an String parameter.

这里让我们简单看一下它的基本实现类，其实也是刚刚`BaseExcutor`中的成员变量 ***PerpetualCache***

```java
public class PerpetualCache implements Cache {

  private final String id;

  private final Map<Object, Object> cache = new HashMap<>();

  public PerpetualCache(String id) {
    this.id = id;
  }
   ....
}
```

可以看到其内部就是通过`String`和`HashMap`来实现的缓存





#### 运行流程



#### 一级缓存使用原理

当进行某一语句查询时，创建`SqlSession`会根据传入的`Statement`来决定执行策略；

当传入***select***的时，会最终执行***selectList***的方法；可以看到最终执行的是`BaseExcutor`的***query***语句方法

```java
public <E> List<E> selectList(String statement, Object parameter, RowBounds rowBounds) {
      MappedStatement ms = configuration.getMappedStatement(statement);
      return executor.query(ms, wrapCollection(parameter), rowBounds, Executor.NO_RESULT_HANDLER);
}
```



在`BaseExcutor`中会首先生成`CacheKey`，而具体的生成方法如下（由于代码太多，仍采用简略模式）：

```java
  public CacheKey createCacheKey(MappedStatement ms, Object parameterObject, RowBounds rowBounds, BoundSql boundSql) {
    if (closed) {
      throw new ExecutorException("Executor was closed.");
    }
    CacheKey cacheKey = new CacheKey();
    cacheKey.update(ms.getId());
    cacheKey.update(rowBounds.getOffset());
    cacheKey.update(rowBounds.getLimit());
    cacheKey.update(boundSql.getSql());
	.....
    return cacheKey;
  }
```

可以看到，与key相关的信息有：**语句的id**，**SQL的偏移量**，**以及实际要执行的sql**



接下来，我们根据上面所说的***BaseExecutor***类的`query`方法中，一探缓存使用的秘密；

（由于代码过长就不进行全部展示了）

```java
public <E> List<E> query(MappedStatement ms, Object parameter, RowBounds rowBounds, ResultHandler resultHandler, CacheKey key, BoundSql boundSql) throws SQLException {
    ...
    try {
      queryStack++;
      list = resultHandler == null ? (List<E>) localCache.getObject(key) : null;
      if (list != null) {
        handleLocallyCachedOutputParameters(ms, key, parameter, boundSql);
      } else {
        list = queryFromDatabase(ms, parameter, rowBounds, resultHandler, key, boundSql);
      }
    } finally {
      queryStack--;
    }
    ...
    return list;
  }
```

会通过`localCache`根据刚刚生成的`key`确认，在当前的***Executor***中，是否有相同key值缓存的存在。

*   如果有则直接处理缓存；
*   否则通过`queryFromDatabase`方法，字面描述，哈哈，从数据库中再次查询；

PS：

[1]只要两条SQL的下列五个值相同，即可以认为是相同的SQL。

>   Statement Id + Offset + Limmit + Sql + Params



#### 一级缓存清理的位置

清理一级缓存的方法

```java
  @Override
  public void clearLocalCache() {
    if (!closed) {
      localCache.clear();
      localOutputParameterCache.clear();
    }
  }
```

*   如果是`insert/delete/update`方法，缓存会刷新。原因是内部他们调用了***clearLocalCache***的方法；
*   在`query`方法执行的最后，会判断一级缓存级别是否是`STATEMENT`级别，如果是的话，就清空缓存，这也就是`STATEMENT`级别的一级缓存无法共享`localCache`的原因；
*   `commit`方法进行事务提交时，也会进行缓存清理；
*   `rollback`方法进行事务回滚时，会对缓存进行清理；
*   `query`的***statement***语句如果再配置中制定了`flushCache=true`，也会进行缓存刷新；
*   当***config.xml***文件中`LocalCacheScope`属性配置为`STATEMENT`，会在`query`查询结束，对缓存进行清理；



### 总结：

1.   一级缓存的使用时Mybaits默认打开的

2.   在使用过程中，可能会造成脏读数据的存在

     >   作者在测试过程中，使用的Mysql数据库-重复读模式，两者混合使用，经常会造成重复读的现象；
     >
     >   在没有日志的情况下，很难分清到底是缓存还是Mysql的引擎特性造成；
     >
     >   由于作者是传统的金融开发从业者，对这种重复读的现象还是很慎重，所以使用时很谨慎。

3.   建议在重要的场景中或者特殊的语句，建议通过`flushCache=true`的参数，将一级缓存进行关闭



鸣谢：

[聊聊MyBatis缓存机制](https://tech.meituan.com/2018/01/19/mybatis-cache.html)

[掘金小册](https://juejin.cn/book/6944917557878980638)

