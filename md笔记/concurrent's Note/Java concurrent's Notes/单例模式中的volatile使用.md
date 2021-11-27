#### 懒汉式的单例模式中，为什么要用volatile关键字呢？



`volatile`关键字有一个重要的功能 - **禁止指令重排序**



以下是使用javap命令生成的JVM指令码

```java
public static com.xxm.singleton.Singleton getInstance();
    Code:
       0: getstatic     #2                  // Field instance:Lcom/xxm/singleton/Singleton;
       3: ifnonnull     37
       6: ldc           #3                  // class com/xxm/singleton/Singleton
       8: dup
       9: astore_0
      10: monitorenter
      11: getstatic     #2                  // Field instance:Lcom/xxm/singleton/Singleton;
      14: ifnonnull     27
      17: new           #3                  // class com/xxm/singleton/Singleton
      20: dup
      21: invokespecial #4                  // Method "<init>":()V
      24: putstatic     #2                  // Field instance:Lcom/xxm/singleton/Singleton;
      27: aload_0
      28: monitorexit
      29: goto          37
      32: astore_1
      33: aload_0
      34: monitorexit
      35: aload_1
      36: athrow
      37: getstatic     #2                  // Field instance:Lcom/xxm/singleton/Singleton;
      40: areturn
    Exception table:
       from    to  target type
          11    29    32   any
          32    35    32   any
```

下面这句代码在上面的指令码中体现在17、20、21、24这连续的4行。

```java
instance = new Singleton();
```

-   17: new #3 // class com/xxm/singleton/Singleton

     代表创建新的对象实例，即创建Singleton类的实例，这个实例是空的，比如有一些属性，那么属性的值都会被设为默认值。

-   20: dup

     代表复制栈顶一个字长的数据，将复制后的数据压栈，不知道到底什么用先忽略。

-   21: invokespecial #4 // Method ““:()V

     代表编译时方法绑定调用方法，即调用init方法，会初始化刚才new的实例。

-   24: putstatic #2 // Field instance:Lcom/xxm/singleton/Singleton;

     代表给静态字段赋值，即将实例化出来的实例赋值给静态属性instance。



**如果21和24调换位置，那么就会发生一些有意思且奇怪的事情~**



新执行顺序

-   17: new #3 // class com/xxm/singleton/Singleton
-   24: putstatic #2 // Field instance:Lcom/xxm/singleton/Singleton;
-   21: invokespecial #4 // Method ““:()V

 当17执行完，此时已经有一个Singleton的实例，然后执行21的给静态属性赋值，那么此时instance属性的就指向了刚实例化的Singleton对象。

问题来了！

**这个对象是半初始化状态的，虽然已经在内存里开辟了空间，但是这个对象的属性还都是默认值啊！！因为给默认值赋值的21还没执行呢！**

**当一个线程争取到锁开始执行这句代码，且刚好指令重排了，这个线程刚执行完24这句指令，突然又来了一个线程获取实例，第一次check时if (instance == null) ，发现不为null，直接返回了！！**



鸣谢：

https://codingxxm.gitee.io/2020/04/11/DCL%E5%8D%95%E4%BE%8B%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E4%BD%BF%E7%94%A8volatile%E5%85%B3%E9%94%AE%E5%AD%97/