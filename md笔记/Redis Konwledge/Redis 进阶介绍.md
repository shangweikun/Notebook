## Redis 使用场景

### 博客文章或者热点文章新闻等访问量计数

类比数字名片的线索信息，可以通过redis实现高效的数据存入，并将高频使用数据分类保存



### 计数器使用

分布式场景中，通过redis内存的高速读写，可以实现计数器的相关操作；

* 利用 `String` 类型；

* 利用自增自减操作；



### 缓存

保证热点数据长期有效的缓存在Redis中，

​	1、通过集群的形势保障数据的高可用；

​	2、设置内存的最大使用量以及淘汰策略来保证缓存的命中率

缓存的缓存条件：

* 高频使用，低频修改；
* 实效性要求低，但访问量较大；



###Session会话缓存

可以用作记录浏览器的回话信息的记录；

* 登陆实效性校验；
* 高频数据缓存。



###并发场景 - 分布式锁

Redis在分布式场景中，由于单线程的单一特性，可以用作分布式锁来保证应用的并发场景。

lock4j的分布式锁中支持redis使用

可以参考文章：

https://blog.csdn.net/lihao21/article/details/49104695

https://www.jianshu.com/p/3582556fe6f1



##Redis 数据淘汰策略

可以设置内存最大使用量，当内存使用量超出时，会施行数据淘汰策略。

Redis的6种数据淘汰策略

|      策略       |                         描述                         |
| :-------------: | :--------------------------------------------------: |
|  volatile-lru   | 从已设置过期时间的数据集中挑选最近最少使用的数据淘汰 |
|  volatile-ttl   |   从已设置过期时间的数据集中挑选将要过期的数据淘汰   |
| volatile-random |      从已设置过期时间的数据集中任意选择数据淘汰      |
|   allkeys-lru   |       从所有数据集中挑选最近最少使用的数据淘汰       |
| allkeys-random  |          从所有数据集中任意选择数据进行淘汰          |
|   noeviction    |                     禁止驱逐数据                     |



[redis官方介绍]:https://redis.io/topics/lru-cache	"参考"

> **noeviction**: return errors when the memory limit was reached and the client is trying to execute commands that could result in more memory to be used (most write commands, but [DEL](https://redis.io/commands/del) and a few more exceptions).
>
> **allkeys-lru**: evict keys by trying to remove the less recently used (LRU) keys first, in order to make space for the new data added.
>
> **volatile-lru**: evict keys by trying to remove the less recently used (LRU) keys first, but only among keys that have an **expire set**, in order to make space for the new data added.
>
> **allkeys-random**: evict keys randomly in order to make space for the new data added.
>
> **volatile-random**: evict keys randomly in order to make space for the new data added, but only evict keys with an **expire set**.
>
> **volatile-ttl**: evict keys with an **expire set**, and try to evict keys with a shorter time to live (TTL) first, in order to make space for the new data added.







##持久化

Redis作为内存型数据库，需要有备用持久化策略来保障数据丢失的可能；



### RDB

将某个时点的数据，存放在硬盘上。

可以快速的复制到其他的服务器，从而创建具有相同数据的服务器副本。



### AOF

将写命令添加到AOF文件（Append Only File）的末尾；使用 AOF 持久化需要设置同步选项，从而确保写命令同步到磁盘文件上的时机。这是因为对文件进行写入并***不会马上将内容同步到磁盘上***，而是***先存储到缓冲区***，然后由操作系统决定什么时候同步到磁盘。有以下同步选项：

|   选项   |         同步频率         |
| :------: | :----------------------: |
|  always  |     每个写命令都同步     |
| everysec |       每秒同步一次       |
|    no    | 让操作系统来决定何时同步 |

- always 选项会严重减低服务器的性能；
- everysec 选项比较合适，可以保证系统崩溃时只会丢失一秒左右的数据，并且 Redis 每秒执行一次同步对服务器性能几乎没有任何影响；
- no 选项并不能给服务器性能带来多大的提升，而且也会增加系统崩溃时数据丢失的数量。





## 事件



###文件事件

服务器通过套接字与客户端或者其它服务器进行通信，文件事件就是对套接字操作的抽象。

Redis 基于  `Reactor 模式` 开发了自己的网络事件处理器，使用 ` I/O 多路复用` 程序来同时监听多个套接字，并将到达的事件传送给文件事件分派器，分派器会根据套接字产生的事件类型调用相应的事件处理器。

<img src="/Users/swk/Pictures/shareP/Redis文件事件.png" alt="Redis文件事件" style="zoom: 67%;" />

[Reactor 模式]: https://zh.wikipedia.org/wiki/%E5%8F%8D%E5%BA%94%E5%99%A8%E6%A8%A1%E5%BC%8F	"维基百科 - 反应器模式"



###时间事件

服务器有一些操作需要在给定的时间点执行，时间事件是对这类定时操作的抽象。

时间事件又分为：

- 定时事件：是让一段程序在指定的时间之内执行一次；
- 周期性事件：是让一段程序每隔指定时间就执行一次。

Redis 将所有时间事件都放在一个无序链表中，通过遍历整个链表查找出已到达的时间事件，并调用相应的事件处理器。



### 事件的调度和执行

服务器需要不断监听文件事件的套接字才能得到待处理的文件事件，但是不能一直监听，否则时间事件无法在规定的时间内执行，因此监听时间应该根据距离现在最近的时间事件来决定。

事件调度与执行由 aeProcessEvents 函数负责，伪代码如下：

```python
def aeProcessEvents():
    # 获取到达时间离当前时间最接近的时间事件
    time_event = aeSearchNearestTimer()
    # 计算最接近的时间事件距离到达还有多少毫秒
    remaind_ms = time_event.when - unix_ts_now()
    # 如果事件已到达，那么 remaind_ms 的值可能为负数，将它设为 0
    if remaind_ms < 0:
        remaind_ms = 0
    # 根据 remaind_ms 的值，创建 timeval
    timeval = create_timeval_with_ms(remaind_ms)
    # 阻塞并等待文件事件产生，最大阻塞时间由传入的 timeval 决定
    aeApiPoll(timeval)
    # 处理所有已产生的文件事件
    procesFileEvents()
    # 处理所有已到达的时间事件
    processTimeEvents()
```

将 aeProcessEvents 函数置于一个循环里面，加上初始化和清理函数，就构成了 Redis 服务器的主函数，伪代码如下：

```python
def main():
    # 初始化服务器
    init_server()
    # 一直处理事件，直到服务器关闭为止
    while server_is_not_shutdown():
        aeProcessEvents()
    # 服务器关闭，执行清理操作
    clean_server()
```



## Redis 主从 哨兵 集群



### 复制

通过使用 slaveof host port 命令来让一个服务器成为另一个服务器的从服务器。

一个从服务器只能有一个主服务器，并且不支持主主复制。

***复制过程：***

1. 主服务器创建快照文件，发送给从服务器，并在发送期间使用缓冲区记录执行的写命令。快照文件发送完毕之后，开始向从服务器发送存储在缓冲区中的写命令；

2. 从服务器丢弃所有旧数据，载入主服务器发来的快照文件，之后从服务器开始接受主服务器发来的写命令；

3. 主服务器每执行一次写命令，就向从服务器发送相同的写命令。

----



### 哨兵（Sentinel）

由一个或多个Sentinel去监听任意多个主服务以及主服务器下的所有从服务器，并在被监视的主服务器进入下线状态时，自动将下线的主服务器属下的某个从服务器升级为新的主服务器，然后由新的主服务器代替已经下线的从服务器，并且Sentinel可以互相监视。



**Redis哨兵的高可用**

![redis](/Users/swk/blog/study/redis.jpeg)

1. 哨兵机制建立了多个哨兵节点(进程)，共同监控数据节点的运行状况。
2. 同时哨兵节点之间也互相通信，交换对主从节点的监控状况。
3. 每隔1秒每个哨兵会向整个集群：Master主服务器+Slave从服务器+其他Sentinel（哨兵）进程，发送一次ping命令做一次心跳检测。

这个就是哨兵用来判断节点是否正常的重要依据，涉及两个新的概念**：主观下线和客观下线。**

**1. 主观下线：**一个哨兵节点判定主节点down掉是主观下线。

**2.客观下线：**只有半数哨兵节点都主观判定主节点down掉，此时多个哨兵节点交换主观判定结果，才会判定主节点客观下线。

**3.原理：**基本上哪个哨兵节点最先判断出这个主节点客观下线，就会在各个哨兵节点中发起投票机制Raft算法（选举算法），最终被投为领导者的哨兵节点完成主从自动化切换的过程。

鸣谢：https://zhuanlan.zhihu.com/p/62947738



redis sentinel vs clustering : https://stackoverflow.com/questions/31143072/redis-sentinel-vs-clustering

---



### 集群（分片）

集群是Redis提供的分布式数据库方案，集群通过分片来进行数据共享，并提供复制和故障转移功能。一个Redis集群通常由多个节点组成；最初，每个节点都是独立的，需要将独立的节点连接起来才能形成可工作的集群。

***使用：***

Cluster Nodes命令和Cluster Meet命令，添加和连接节点形成集群。



####***分片：***

分片是将数据划分为多个部分的方法，可以将数据存储到多台机器里面，这种方法在解决某些问题时可以获得线性级别的性能提升。

假设有 4 个 Redis 实例 R0，R1，R2，R3，还有很多表示用户的键 user:1，user:2，... ，有不同的方式来选择一个指定的键存储在哪个实例中。

- 最简单的方式是范围分片，例如用户 id 从 0\~1000 的存储到实例 R0 中，用户 id 从 1001\~2000 的存储到实例 R1 中，等等。但是这样需要维护一张映射范围表，维护操作代价很高。
- 还有一种方式是哈希分片，使用 CRC32 哈希函数将键转换为一个数字，再对实例数量求模就能知道应该存储的实例。

根据执行分片的位置，可以分为三种分片方式：

- 客户端分片：客户端使用一致性哈希等算法决定键应当分布到哪个节点。
- 代理分片：将客户端请求发送到代理上，由代理转发请求到正确的节点上。
- 服务器分片：Redis Cluster。





### **15.假如Redis里面有1亿个key，其中有10w个key是以某个固定的已知的前缀开头的，如果将它们全部找出来？**

使用keys指令可以扫出指定模式的key列表。

**对方接着追问：如果这个redis正在给线上的业务提供服务，那使用keys指令会有什么问题？**

这个时候你要回答redis关键的一个特性：redis的单线程的。keys指令会导致线程阻塞一段时间，线上服务会停顿，直到指令执行完毕，服务才能恢复。这个时候可以使用scan指令，scan指令可以无阻塞的提取出指定模式的key列表，但是会有一定的重复概率，在客户端做一次去重就可以了，但是整体所花费的时间会比直接用keys指令长。



### **17.如果有大量的key需要设置同一时间过期，一般需要注意什么？**

如果大量的key过期时间设置的过于集中，到过期的那个时间点，redis可能会出现短暂的卡顿现象。一般需要在时间上加一个随机值，使得过期时间分散一些。



### **19.Pipeline有什么好处，为什么要用pipeline？**

可以将多次IO往返的时间缩减为一次，前提是pipeline执行的指令之间没有因果相关性。使用redis-benchmark进行压测的时候可以发现影响redis的QPS峰值的一个重要因素是pipeline批次指令的数目。



面试宝典：https://zhuanlan.zhihu.com/p/130923806