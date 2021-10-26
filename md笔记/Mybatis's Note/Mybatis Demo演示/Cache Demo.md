### 一级缓存 重复读问题

一级缓存脏读说明，在一个session周期中，如果相同的sql和相同的查询条件执行，会通过Cache进行结果缓存；

如果在期间有其他事务更新了数据库，会导致最新的查询结果无法得到，从而导致 **重复读**



环境及代码说明：

数据库：`MYSQL8.0`；框架：`Mybatis3.5.7`；

代码：1、自动事务提交；2、`session` 中查询 **Users** 表两次；3、`innerSession` 在 `session` 的生命周期中执行一次更新；

```java
public static void repeatReadFirstCache() {
    _WORKER.workAutoCommit(session -> {
        User user = UserMethod.selectInfo(session.getMapper(UserMapper.class), 1);
        System.out.println("session:" + user);

        _WORKER.workAutoCommit(innerSession -> {
            System.out.println("innerSession:"
                               + UserMethod.updateInfo(innerSession.getMapper(UserMapper.class),
                                                       FirstCache.getPhone(user), 1));
        });

        System.out.println("search twice:");
        System.out.println("session:" + UserMethod.selectInfo(session.getMapper(UserMapper.class), 1));
    });
}
```



```log
Logging initialized using 'class org.apache.ibatis.logging.stdout.StdOutImpl' adapter.
PooledDataSource forcefully closed/removed all connections.
PooledDataSource forcefully closed/removed all connections.
PooledDataSource forcefully closed/removed all connections.
PooledDataSource forcefully closed/removed all connections.
Cache Hit Ratio [com.example.dao.UserMapper]: 0.0
Opening JDBC Connection
Created connection 1391067753.
==>  Preparing: select * from users where id = ?
==> Parameters: 1(Integer)
<==    Columns: id, name, phone
<==        Row: 1, ni hao, d5616fbfcb1697aaeffc3432495906e3
<==      Total: 1
session:User(id=1, name=UserName(name=你好), phone=ClientPhone(phone=13189783309))
Opening JDBC Connection
Created connection 841894382.
Setting autocommit to false on JDBC Connection [com.mysql.cj.jdbc.ConnectionImpl@322e49ee]
==>  Preparing: update users set phone = ? where id = ?
==> Parameters: 559710330a1b9ff5bdabe7c65c2767cb(String), 1(Integer)
<==    Updates: 1
innerSession:1
Committing JDBC Connection [com.mysql.cj.jdbc.ConnectionImpl@322e49ee]
Resetting autocommit to true on JDBC Connection [com.mysql.cj.jdbc.ConnectionImpl@322e49ee]
Closing JDBC Connection [com.mysql.cj.jdbc.ConnectionImpl@322e49ee]
Returned connection 841894382 to pool.
search twice:
Cache Hit Ratio [com.example.dao.UserMapper]: 0.0
session:User(id=1, name=UserName(name=你好), phone=ClientPhone(phone=13189783309))
Closing JDBC Connection [com.mysql.cj.jdbc.ConnectionImpl@52ea0269]
Returned connection 1391067753 to pool.

Process finished with exit code 0
```

