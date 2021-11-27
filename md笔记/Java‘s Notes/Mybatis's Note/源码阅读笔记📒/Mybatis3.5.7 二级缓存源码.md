### 使用

1.   在 mapper.xml 上打一个 `<cache />` 标签，就算开启二级缓存了：

     ```xml
     <?xml version="1.0" encoding="UTF-8" ?>
     <!DOCTYPE mapper
             PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
             "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
     <mapper namespace="com.linkedbear.mybatis.mapper.DepartmentMapper">
         <cache />
         <!-- ...... -->
     </mapper>
     ```

     

2.   在config.xml文件中，setting标签中的cacheEnable配置，是二级缓存的总开关

     ```xml
         <settings>
             <setting name="cacheEnabled" value="true"/>
             <!-- ...... -->
         </settings>
     ```



3.   在select标签中的 useCache属性，默认值是true，是控制该sql查询是否使用二级缓存

     ```xml
         <select id="selectUserById" resultType="com.example.entity.User" useCache="true">
             <!-- ...... -->
         </select>
     ```



#### 可选择属性

| 属性          | 描述                             | 备注                                                         |
| ------------- | -------------------------------- | ------------------------------------------------------------ |
| eviction      | 缓存的回收策略                   | 默认 LRU                                                     |
| type          | 二级缓存的实现                   | 默认 `org.apache.ibatis.cache.impl.PerpetualCache` ，即本地内存的二级缓存 |
| size          | 缓存引用数量                     | 默认值 1024                                                  |
| flushInterval | 缓存刷新间隔（定时清除时间间隔） | 默认无，即没有刷新间隔                                       |
| readOnly      | 缓存是否只读                     | 默认 false ，需要二级缓存对应的实体模型类需要实现 `Serializable` 接口 |
| blocking      | 阻塞获取缓存数据                 | 若缓存中找不到对应的 key ，是否会一直 blocking ，直到有对应的数据进入缓存。默认 false |





### 二级缓存的设计

其中核心的类为`Cache` 

参考下图是mybatis3.5.7中的类的实现类，可以看到其中真正的实现类只有`PerpetualCache`，其他都是装饰器模式的应用；

![一级缓存图片](http://42.193.118.12/images/Cache‘s ImplAndDecorators.png)



#### PerpetualCache

其内部是通过HashMap对缓存进行存储的





### 二级缓存实现原理



*   核心的`CachingExecutor`类的query方法

    下面是源码环节

    ```java
    @Override
    public <E> List<E> query(MappedStatement ms, Object parameterObject, RowBounds rowBounds, ResultHandler resultHandler) throws SQLException {
        BoundSql boundSql = ms.getBoundSql(parameterObject);
        CacheKey key = createCacheKey(ms, parameterObject, rowBounds, boundSql);
        return query(ms, parameterObject, rowBounds, resultHandler, key, boundSql);
    }
    
    @Override
    public <E> List<E> query(MappedStatement ms, Object parameterObject, RowBounds rowBounds, ResultHandler resultHandler, CacheKey key, BoundSql boundSql)
        throws SQLException {
        Cache cache = ms.getCache();
        if (cache != null) {
            flushCacheIfRequired(ms);
            if (ms.isUseCache() && resultHandler == null) {
                ensureNoOutParams(ms, boundSql);
                @SuppressWarnings("unchecked")
                List<E> list = (List<E>) tcm.getObject(cache, key);
                if (list == null) {
                    list = delegate.query(ms, parameterObject, rowBounds, resultHandler, key, boundSql);
                    tcm.putObject(cache, key, list);
                }
                return list;
            }
        }
        return delegate.query(ms, parameterObject, rowBounds, resultHandler, key, boundSql);
    }
    ```

    1.   首先，获取`BoundSql`，并同时根据`MappedStatement`、上送参数、`RowBounds`以及刚刚获取的`BoundSql`生成`CacheKey`；
    2.   其次，确认该`MappedStatement`是否开启了二级缓存；如果没有，直接调用数据库的底层查询；
    3.   再次之，检查该`MappedStatement`是否要求刷新缓存；检查是否使用了`userCache`，以及未使用`ResultHandle`；
    4.   然后，检查绑定语句是否未使用存储过程等的输出参数；如果存在，则报错处理；
    5.   最后，查询缓存；未命中则调用`delegate`的查询方法，其中包含了从一级缓存和数据库中查询。

    

*   为了模拟数据库的`read committed`模式，**二级缓存会在事务提交后，才进行最终缓存操作**



*   需要提醒大家的一点就是，当`update`标签属性`flushCache`设置未`false`时，**一级二级缓存都不会主动进行刷新操作**；

