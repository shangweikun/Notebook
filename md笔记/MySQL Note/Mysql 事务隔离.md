### 什么是事务？

事务，由一个有限的数据库操作序列构成，这些操作要么全部执行,要么全部不执行，是一个不可分割的工作单位。



#### 四大特性

**原子性：** 事务作为一个整体被执行，包含在其中的对数据库的操作要么全部都执行，要么都不执行。

**一致性：** 指在事务开始之前和事务结束以后，数据不会被破坏，假如A账户给B账户转10块钱，不管成功与否，A和B的总金额是不变的。

**隔离性：** 多个事务并发访问时，事务之间是相互隔离的，一个事务不应该被其他事务干扰，多个并发事务之间要相互隔离。。

**持久性：** 表示事务完成提交后，该事务对数据库所作的操作更改，将持久地保存在数据库之中。





#### 三大现象

**脏读 *Dirty reads*** ：A事务在修改过程中（事务未提交或结束），B事务可以查看到A事务所修改的数据记录

>   A *dirty read* (aka *uncommitted dependency*) occurs when a transaction is allowed to read data from a row that has been modified by another running transaction and not yet committed.

**不可重复读 *Non-repeatable reads***：**在一个事务的两次查询**中数据不一致，可能是两次查询过程中另一个事务更新了数据。

>   A *non-repeatable read* occurs when, during the course of a transaction, a row is retrieved twice and the values within the row differ between reads.

**幻读 *Phantom reads***：一个事务的两次查询中数据不一致。例如一个事务查询数据，而另一个事务却插入新的数据，先前事务的查询中，发现一些数据是之前查询中没有的。

>   A *phantom read* occurs when, in the course of a transaction, new rows are added or removed by another transaction to the records being read.



##### 脏读：

| 事务A                                                        | 事务B                                                        |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| /* Query 1 */ `SELECT age FROM users WHERE id = 1;` <br/>/\* will read 20 */ |                                                              |
|                                                              | /* update */ `UPDATE users SET age = 21 WHERE id = 1; `<br/>/\* No commit here */ |
| /* Query 2 */ `SELECT age FROM users WHERE id = 1;` <br/>/\* will read 21 */ |                                                              |
|                                                              | `ROLLBACK;` /* lock-based DIRTY READ */                      |



##### 不可重复读：

| 事务A                                                        | 事务B                                                        |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| /* Query 1 */ `SELECT * FROM users WHERE id = 1;`            |                                                              |
|                                                              | /* Query 2 */<br/>`UPDATE users SET age = 21 WHERE id = 1; COMMIT; `<br/>/\* in multiversion concurrency    control, or lock-based READ COMMITTED */ |
| /* Query 1 */ `SELECT * FROM users WHERE id = 1; COMMIT; `**<br/>**/\* lock-based REPEATABLE READ */ |                                                              |



##### 幻读：

| 事务A                                                        |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Transaction 1                                                | Transaction 2                                                |
| /* Query 1 */ <br/>`SELECT * FROM users WHERE age BETWEEN 10 AND 30; ` |                                                              |
|                                                              | /* Insert 2 */<br/> `INSERT INTO users(id, name, age) VALUES (3, 'Bob', 27); COMMIT; ` |
| /* Query 1 */ <br/>`SELECT * FROM users WHERE age BETWEEN 10 AND 30; COMMIT; ` |                                                              |





### `MYSQL`的事务的四大隔离级别

既然并发事务存在**脏读、不可重复、幻读**等问题，`InnoDB`实现了几种事务的隔离级别应对

-   读未提交（`Read Uncommitted`）
    -   读取未提交内容，所有事务可看到其他未提交事务的结果，很少实际使用
    -   读取未提交的数据称为脏读（Dirty Read）
-   读已提交（`Read Committed`）
    -   多数数据库的默认隔离级别（`MySQL`默认不是，默认为`Repeatable Read`）
    -   满足隔离的简单定义：**一个事务只能看到已提交事务所做的改变**
    -   这种隔离级别，支持所谓的不可重读（Non-repeatable Read），同一事务的其他实例在该实例过程中可能有新commit，所以同一个select可能返回不同结果（**同一个事务如何做到其他实例？**）
-   可重复读（`Repeatable Read`）
    -   可重复读(`MySQL`默认事务隔离)，但可能出现幻读(Phantom Read)
    -   `InnoDB`通过多版本并发控制`MVCC`机制解决该问题
    -   PS：新版`MySQL`采用`Next-Key锁`来解决幻读问题
-   串行化（`Serializable`）
    -   最高隔离级别，强制事务排序（串行化），不会互相冲突
    -   每个读数据航增加共享锁
    -   此级别，可能导致大量超时现象和锁竞争





### `MySql`隔离级别的实现原理

实现隔离机制的方法主要有两种：

-   读写锁
-   一致性快照读，即 `MVCC`

`MySql`使用不同的锁策略(Locking Strategy)/`MVCC`来实现四种不同的隔离级别。



#### MVCC的实现原理

`MVCC(Multi-Version Concurrency Control)`，中文叫**多版本并发控制**，它是通过读取历史版本的数据，来降低并发事务冲突，从而提高并发性能的一种机制。

它的实现依赖于**隐式字段、undo日志、快照读&当前读、Read View**。



##### 隐式字段

对于`InnoDB`存储引擎，每一行记录都有两个隐藏列**`DB_TRX_ID`、`DB_ROLL_PTR`**;

如果表中没有主键和非NULL唯一键时，则还会有第三个隐藏的主键列**`DB_ROW_ID`**。

-   `DB_TRX_ID`，记录每一行最近一次修改（修改/更新）它的事务ID，大小为6字节；
-   `DB_ROLL_PTR`，这个隐藏列就相当于一个指针，指向回滚段的undo日志，大小为7字节；
-   `DB_ROW_ID`，单调递增的行ID，大小为6字节；



##### undo日志

多个事务并行操作某一行数据时，不同事务对该行数据的修改会产生多个版本;

然后通过回滚指针（`DB_ROLL_PTR`）连一条**Undo日志链**。

>事务未提交的时候，修改数据的镜像（修改前的旧版本），存到undo日志里。以便事务回滚时，恢复旧版本数据，撤销未提交事务数据对数据库的影响。

>   undo日志是逻辑日志。可以这样认为，当delete一条记录时，undo log中会记录一条对应的insert记录，当update一条记录时，它记录一条对应相反的update记录。

>   存储undo日志的地方，就是**回滚段**。



##### 快照读&当前读

**快照读：**读取的是记录数据的可见版本（有旧的版本），不加锁,普通的select语句都是快照读,如：

**当前读：**读取的是记录数据的最新版本，显示加锁的都是当前读

```sql
select * from account where id>2 lock in share mode;
select * from  account where id>2 for update;
```



##### Read View

-   Read View就是事务执行**快照读**时，产生的读视图。
-   事务执行快照读时，会生成数据库系统当前的一个快照，记录当前系统中还有哪些活跃的读写事务，把它们放到一个列表里。
-   Read View主要是用来做可见性判断的，即判断当前事务可见哪个版本的数据~

为了下面方便讨论Read View可见性规则，先定义几个变量

>   -   `m_ids`:当前系统中那些活跃的读写事务ID,它数据结构为一个List。
>   -   `min_limit_id:m_ids`事务列表中，最小的事务ID
>   -   `max_limit_id:m_ids`事务列表中，最大的事务ID

-   如果`DB_TRX_ID `< `min_limit_id`，表明生成该版本的事务在生成`ReadView`前已经提交(因为事务ID是递增的)，所以该版本可以被当前事务访问。
-   如果`DB_TRX_ID `> `m_ids`列表中最大的事务id，表明生成该版本的事务在生成`ReadView`后才生成，所以该版本不可以被当前事务访问。
-   如果 `min_limit_id `=<`DB_TRX_ID`<= `max_limit_id`,需要判断`m_ids.contains(DB_TRX_ID)`;
    -   如果在，则代表`Read View`生成时刻，这个事务还在活跃，还没有Commit，你修改的数据，当前事务也是看不见的；
    -   如果不在，则说明，你这个事务在`Read View`生成之前就已经Commit了，修改的结果，当前事务是能看见的。



**注意啦！！** RR跟RC隔离级别，最大的区别就是：

**RC每次读取数据前都生成一个`ReadView`，而RR只在第一次读取数据时生成一个`ReadView`**。



鸣谢：

https://en.wikipedia.org/wiki/Isolation_(database_systems)

https://juejin.cn/post/6844904115353436174

https://fivezh.github.io/2019/02/01/MySQL-Transaction-Isolation-Level/

