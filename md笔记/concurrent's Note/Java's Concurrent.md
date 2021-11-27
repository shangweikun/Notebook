# JAVA

java的FileLock文件🔒

思考：是否可以通过 `锁` 及 `持久化的数据` ，来减小锁的细粒度呢？



##Semaphore 信号量

```java
public final boolean releaseShared(int arg) {
    if (tryReleaseShared(arg)) {
        signalNext(head);
        return true;
    }
    return false;
}
```

以上是`release`的源码：当执行该语句时，会同时释放下一个队列中等待的`waiter`

错误的实例代码如下：

```java
static {
  semaphore2.release();
	semaphore2.notifyAll();    //error 该方法为Object的顶级方法，无需该方法唤醒等待线程
}
```

经典使用的leetcode题目：

#### [1115. 交替打印FooBar](https://leetcode-cn.com/problems/print-foobar-alternately/)





### 不同线程向同一个线程池提交任务，为什么能保证原子性？

#### 1、当任务小于核心数时

通过ReentrantLock，保证添加worker的原子性；

```java
private boolean addWorker(Runnable firstTask, boolean core) {
        retry:
        for (int c = ctl.get();;) {
            // Check if queue empty only if necessary.
            if (runStateAtLeast(c, SHUTDOWN)
                && (runStateAtLeast(c, STOP)
                    || firstTask != null
                    || workQueue.isEmpty()))
                return false;

            for (;;) {
                if (workerCountOf(c)
                    >= ((core ? corePoolSize : maximumPoolSize) & COUNT_MASK))
                    return false;
                if (compareAndIncrementWorkerCount(c))
                    break retry;
                c = ctl.get();  // Re-read ctl
                if (runStateAtLeast(c, SHUTDOWN))
                    continue retry;
                // else CAS failed due to workerCount change; retry inner loop
            }
        }

        boolean workerStarted = false;
        boolean workerAdded = false;
        Worker w = null;
        try {
            w = new Worker(firstTask);
            final Thread t = w.thread;
            if (t != null) {
                final ReentrantLock mainLock = this.mainLock;
                mainLock.lock();
                try {
                    // Recheck while holding lock.
                    // Back out on ThreadFactory failure or if
                    // shut down before lock acquired.
                    int c = ctl.get();

                    if (isRunning(c) ||
                        (runStateLessThan(c, STOP) && firstTask == null)) {
                        if (t.getState() != Thread.State.NEW)
                            throw new IllegalThreadStateException();
                        workers.add(w);
                        workerAdded = true;
                        int s = workers.size();
                        if (s > largestPoolSize)
                            largestPoolSize = s;
                    }
                } finally {
                    mainLock.unlock();
                }
                if (workerAdded) {
                    t.start();
                    workerStarted = true;
                }
            }
        } finally {
            if (! workerStarted)
                addWorkerFailed(w);
        }
        return workerStarted;
    }
```

#### 2、当任务提交到阻塞队列时，通过阻塞队列来保证原子性

`workQueue.offer(command)` 为提交的根本方法，在此处保证原子性即可

```java
public void execute(Runnable command) {
        if (command == null)
            throw new NullPointerException();
        /*
         * Proceed in 3 steps:
         *
         * 1. If fewer than corePoolSize threads are running, try to
         * start a new thread with the given command as its first
         * task.  The call to addWorker atomically checks runState and
         * workerCount, and so prevents false alarms that would add
         * threads when it shouldn't, by returning false.
         *
         * 2. If a task can be successfully queued, then we still need
         * to double-check whether we should have added a thread
         * (because existing ones died since last checking) or that
         * the pool shut down since entry into this method. So we
         * recheck state and if necessary roll back the enqueuing if
         * stopped, or start a new thread if there are none.
         *
         * 3. If we cannot queue task, then we try to add a new
         * thread.  If it fails, we know we are shut down or saturated
         * and so reject the task.
         */
        int c = ctl.get();
        if (workerCountOf(c) < corePoolSize) {
            if (addWorker(command, true))
                return;
            c = ctl.get();
        }
        if (isRunning(c) && workQueue.offer(command)) {
            int recheck = ctl.get();
            if (! isRunning(recheck) && remove(command))
                reject(command);
            else if (workerCountOf(recheck) == 0)
                addWorker(null, false);
        }
        else if (!addWorker(command, false))
            reject(command);
    }
```





### 为什么线程间使用单一实例或者静态方法，不会造成阻塞？

- ##### volatile

  多线程的内存模型：main memory（主存）、working memory（线程栈），在处理数据时，线程会把值从主存load到本地栈，完成操作后再save回去(volatile关键词的作用：每次针对该变量的操作都激发一次load and save)。

![img](https://upload-images.jianshu.io/upload_images/1689841-d4ab6cfda7042c67.png?imageMogr2/auto-orient/strip%7CimageView2/2)

volatile

针对多线程使用的变量如果不是volatile或者final修饰的，很有可能产生不可预知的结果（另一个线程修改了这个值，但是之后在某线程看到的是修改之前的值）。其实道理上讲同一实例的同一属性本身只有一个副本。但是多线程是会缓存值的，本质上，volatile就是不去缓存，直接取值。在线程安全的情况下加volatile会牺牲性能。

PS:如果方法涉及操作同一实例或静态类的参数值，请使用***同步方式***或者***并发容器***



###Thread.isAlive状态中，`BOLCK`非存活状态。



### Java多线程，如何实现静态类和单一实例的共享呢？

​      java在执行静态方法时，会在内存中拷贝一份，如果静态方法所在的类里面没有静态的变量，那么线程访问就是安全的，比如在javaee中服务器必然会多线程的处理请求此时如果设计全局需要调用的静态方法，可用此种设计。



###多线程间的Heap和Stack的使用情况

进程创建线程，每个线程可以共享进程的地址空间；但同时线程需要保留一些自己私有的数据

unix中的thread独自持有的资源：

- Stack pointer
- Registers
- scheduling properties(policy and priority)
- set of pending and blocked signals
- Thread specific data

线程操作的特点：

- Changes made by one thread to shared system resources will be seen by all other threads
- Two pointers(may belong by different threads) have the same value point to the same data
- Reading and Writing to the same memory locations need explicit synchronization by programmer

使用线程的优势：

- Light weight: can be created with less overhead(process: fork(); thread: pthread_creat())
- Efficient communication / Data exchange(not copy data opration, just need to pass address)

一个进程创建的多个线程：每个线程都拥有自己私有的Stack，但共享一个Heap

这样做的原因 ：

（1）stack is for local/method variables；  heap is for instance/class variable

（2）Stack常常用来存放 函数的参数，函数中使用的自动变量，存放过程活动记录；如果多个线程共享一个Stack

会非常的凌乱，不方便使用

（3）使用共享Heap的目的是为了高效的数据共享

线程间的数据交换有两种方式：

（1）共享内存方式shared memory（共享堆）：最大的优势是快速

（2）消息传递方式message passing（不需要共享堆）：优势在于安全

鸣谢：https://blog.csdn.net/yyf_it/article/details/49924157