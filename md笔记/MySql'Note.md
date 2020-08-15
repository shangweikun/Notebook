#MySql'Note



# 常用语句

```sql
show processlist;

show variables like 'transaction_isolation';--查看“事务隔离级别”
select * from information_schema.innodb_trx where TIME_TO_SEC(timediff(now(),trx_started))>60;
--查询持续时间超过 60s 的事务



```



##Mysql两大功能模块

两阶段提交，**[prepare]** 和 **[commit]** 状态

一条语句的执行，需要依次连接器、分析层/优化器/执行器、缓存【mysql已经取消】

（思考：交由持久型数据库来实现缓存，会导致频繁的刷新缓存，造成额外资源开销；故衍生***\*redis等的缓存中间件\****来实现功能）



###Service

该层次包含了大多数的mysql的功能，包括函数，即上述的一些功能器

binlog日志：最后**【提交完毕】**，登记



###引擎层

redo log日志

两个关键点：**\*write pos\***   **\*check point\***







##Mysql事务隔离级别

###r.u.

### r.c.

### r.r.

### s.(read 和 write 串行化)



回滚日志的处理；

其***【前置】***无长事务绑定的read-view，可以进行清理；





## MySQL索引

mysql中目前常见的的索引为B+树，

其他的有：HashTable[等值查询]，有序数组[LogN的查询效率，支持区间查，更新代价大]，二叉树[高度过高，交互过多，浪费大量磁盘存储空间]

InnoDB引擎中的N叉树，N的值为1200；（**每个索引节点一般都是操作系统页的整数倍**）



### 采用B+树的原因：

减少与磁盘的交互次数（减少树的高度）



### 自增主键

非主键的索引的叶子节点为自增主键，可以有效减小索引的存储空间

<p style="font-family:verdana;color:red">例如身份证号码和自增ID</p>

**PS**：重建主键的索引，相当于是重建表，<strong>所有的非主键索引都会失效</strong>



### 索引中的三种概念

引入一层概念***\*回表\****：回到索引树的查询过程，叫做一次回表



#### 覆盖索引

索引中**【覆盖】**了需要的查询信息，无需再到每一行中去取出数据（例如：非主键索引的叶子节点是主键ID，如果通过非主键索引去查找对应的主键ID值，无需去磁盘中取出每一行的记录，查询到索引树的叶子🍃节点即可）；

一般在考虑时，需要平衡业务-冗余索引的关系；



#### B+的最左前缀

匹配组合索引的最左索引前缀（ps：在设计索引时，需要考虑到索引中的字段使用频率）

组合索引是按照索引的顺序进行排序的，如（a，b）索引，先按照a排序，后按照b进行排序



#### 索引下堆

可以根据条件中的原则，可以提前过滤不满足的数据情况，减少回表次数





## MySQL中的锁🔒

### 全局锁

全局锁使用频率不高，锁住后，所有的操作都会被block

command ：***\*Flush tables with read lock (FTWRL)\****

ps:官方自带的逻辑备份工具是 ***\*mysqldump\**** - 使用参数 ***\*–single-transaction\**** 的时 候，导数据之前就会启动一个事务



### 表级锁

command：***\*表锁的语法是 lock tables ... read/write\****

**MDL(metadata lock)**	元数据锁

当给表增加字段时，元数据锁为block **alter**后的所有操作语句（如果在大量交易的情况下处理，会阻塞导致生产问题）

解决方案：在alter语句中增加***\*wait N\**** 的等待时间



<p style="color:green">在使用工具备份时发生DDL操作时可能出现的情况</p>

```SQL
--Q1:
SET SESSION TRANSACTION ISOLATION LEVEL REPEATABLE READ; 2 Q2:START TRANSACTION WITH CONSISTENT SNAPSHOT;
/* other tables */
--Q3:
SAVEPOINT sp;
/* 时刻 1 */
--Q4:
show create table `t1`; 7 /* 时刻 2 */
--Q5:
SELECT * FROM `t1`;
/* 时刻 3 */
Q6:ROLLBACK TO SAVEPOINT sp; 11 /* 时刻 4 */
/* other tables */
```

参考答案：

1. 如果在 Q4 语句执行之前到达，现象:没有影响，备份拿到的是 DDL 后的表结构。
2. 如果在“时刻 2”到达，则表结构被改过，Q5 执行的时候，报 Table definition has changed, please retry transaction，现象:mysqldump 终止;
3. 如果在“时刻 2”和“时刻 3”之间到达，mysqldump 占着 t1 的 MDL 读锁，binlog 被 阻塞，现象:主从延迟，直到 Q6 执行完成。
4. 从“时刻 4”开始，mysqldump 释放了 MDL 读锁，现象:没有影响，备份拿到的是 DDL 前的表结构。





##行锁

***\*两阶段锁\****：在需要时，添加行锁，在不需要时[事务提交完毕]释放行锁

###死锁处理

#### 设置超时时间

MySQL中有自定义的超时设置参数：***\*innodb_lock_wait_timeout\****(默认值为50)



#### 业务层面屏蔽

确保在业务层面保证不会出现死锁现象

#### 控制并法度

修改MySql的源码，在进入InnoDB引擎前进行排队，强制减少并发程度