### SqlSession.commit原理

测试代码如下：

```java
public class App {

    public static void main(String[] args) throws IOException {

        String resource = "mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);

        try(SqlSession session = sqlSessionFactory.openSession(ExecutorType.BATCH)){
            UserMapper mapper = session.getMapper(UserMapper.class);
            mapper.insertUser(User.builder().id(1).name("ZhangSan").build());
            session.commit();
        }catch (Exception e){
            e.printStackTrace();
        }

    }
}
```



以下时SqlSession的部分源码信息

```java
  @Override
  public void commit(boolean force) {
    try {
      executor.commit(isCommitOrRollbackRequired(force));
      dirty = false;
    } catch (Exception e) {
      throw ExceptionFactory.wrapException("Error committing transaction.  Cause: " + e, e);
    } finally {
      ErrorContext.instance().reset();
    }
  }

  private boolean isCommitOrRollbackRequired(boolean force) {
    return (!autoCommit && dirty) || force;
  }

  @Override
  public int update(String statement, Object parameter) {
    try {
      dirty = true;
      MappedStatement ms = configuration.getMappedStatement(statement);
      return executor.update(ms, wrapCollection(parameter));
    } catch (Exception e) {
      throw ExceptionFactory.wrapException("Error updating database.  Cause: " + e, e);
    } finally {
      ErrorContext.instance().reset();
    }
  }
```

*   `isCommitOrRollbackRequired` 中：
    *   `autoCommit` 是控制自动提交开关
    *   `dirty` 控制脏读开关（当有进行 `update` 操作时，该标志位会被修改为 `true`）
*   更新完毕，脏读开关调整为 `false`





###SqlSession的 autoCommit的实现

测试代码如下：

```java
public class App {

    public static void main(String[] args) throws IOException {

        String resource = "mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);

        try(SqlSession session = sqlSessionFactory.openSession(ExecutorType.BATCH, true)){
            UserMapper mapper = session.getMapper(UserMapper.class);
            mapper.insertUser(User.builder().id(1).name("ZhangSan").build());
            session.commit();, 
        }catch (Exception e){
            e.printStackTrace();
        }
    }
}
```

较之上面的例子，仅仅是在 `openSession` 中的 `autoCommit` 参数设置为 `true`

Mybatis3.5.7版本下，经测试发现，`BATCH` 的批量模式，`autoCommit`参数是设置不生效；





### `autoCommit `原理

*   首先构建过程中，将 `autoCommit` 设置为`true`

```java
private SqlSession openSessionFromDataSource(ExecutorType execType, TransactionIsolationLevel level, boolean autoCommit) {
    Transaction tx = null;
    try {
      final Environment environment = configuration.getEnvironment();
      final TransactionFactory transactionFactory = getTransactionFactoryFromEnvironment(environment);
      tx = transactionFactory.newTransaction(environment.getDataSource(), level, autoCommit);
      final Executor executor = configuration.newExecutor(tx, execType);
      return new DefaultSqlSession(configuration, executor, autoCommit);
    } catch (Exception e) {
      closeTransaction(tx); // may have fetched a connection so lets call close()
      throw ExceptionFactory.wrapException("Error opening session.  Cause: " + e, e);
    } finally {
      ErrorContext.instance().reset();
    }
  }
```

注意，此时 `autoCommit` 的变量被使用了两次

```java
tx = transactionFactory.newTransaction(environment.getDataSource(), level, autoCommit);
```

```java
return new DefaultSqlSession(configuration, executor, autoCommit);
```

*   第一个位置 `Transaction` 这里的实例类为`org.apache.ibatis.transaction.jdbc.JdbcTransaction` ；

    该类的主要功能，是负责创建并维护数据库的连接对象 `java.sql.Connection` ,在此处创建连接时，`autoCommit` 属性会传递给 `Connection` 的实例对象，以便达到预期的自动提交效果；

*   第二个位置的则是 `DefaultSqlSession` 的属性`autoCommit` 的赋值；

    该属性的功能，主要是确认在没有自动提交事务时，对数据库事务进行提交；具体使用的位置如下：

    *   主动声明式提交
    *   主动声明式回滚
    *   在 `sqlSession `关闭时，被动回滚



具体细节，大家可以翻看源码