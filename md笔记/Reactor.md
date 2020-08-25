# 什么是 Reactor?

现在你要了解下 Reactor，不妨在你喜欢的搜索引擎里输入 *Reactive，Spring+Reactive，Asynchronous+Java* 之类的关键词，或者直接输入 *Reactor是什么货？*。简单说，Reactor 是一个轻量级 JVM 基础库，帮助你的服务或应用**高效，异步**地传递消息。

"高效"是指什么?

- 消息从A传递到B时，产生很少的**内存**垃圾，甚至不产生。
- 解决消费者处理消息的效率低于生产者时带来的**溢出**问题。
- 尽可能提供非阻塞**异步流**。



```java
private ExecutorService  threadPool = Executors.newFixedThreadPool(8);

final List<T> batches = new ArrayList<T>();

Callable<T> t = new Callable<T>() { // *1

        public T run() {
                synchronized(batches) { // *2
                        T result = callDatabase(msg); // *3
                        batches.add(result);
                        return result;
                }
        }
};

Future<T> f = threadPool.submit(t); // *4
T result = f.get() // *5
```

1. Callable 分配 -- 可能导致 GC 压力。
2. 同步过程强制每个线程执行停-检查操作。
3. 消息的消费可能比生产慢。
4. 使用线程池(ThreadPool)将任务传递给目标线程 -- 通过 FutureTask 方式肯定会产生 GC 压力。
5. 阻塞直至 callDatabase() 回调。