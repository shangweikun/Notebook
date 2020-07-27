# Java基础

## 一、基础类型

1. boolean / 0|1;

2. byte / 8bit / -127 ~ +127;
3. short / 16bit(2byte)  / -2^15 ~ +2^15;
4. int / 32bit(4byte) / -2^31 ~ +2^31;
5. long / 64bit(8byte) / -2^63 ~ +2^63;
6. char / 16bit(2byte) / 0 ~ 
7. float / 32bit
8. double / 64bit

**ps：局部变量区（与 操作数栈 一起组成 ‘JAVA解释栈桢’）**

​		**数据占据4byte[32bit HotSpot]（除：long和double 占据8byte）**



#### 封装类型

自动拆箱和自动装箱

使用了缓存池 Integer.valueOf	具体实现：

```java
public static Integer valueOf(int i) {
        if (i >= IntegerCache.low && i <= IntegerCache.high)
            return IntegerCache.cache[i + (-IntegerCache.low)];
        return new Integer(i);
    }
```



 * 缓存池

   从初始化的池中进行获取对象	例如：IntegerCache

   ```java
   private static class IntegerCache {
           static final int low = -128;
           static final int high;
           static final Integer cache[];
     
     			static{
             /*···见下···*/
           }
     
           private IntegerCache() {}
       }
   ```

   可参数配置上限大小；

   ```java
   static {
               // high value may be configured by property
               int h = 127;
               String integerCacheHighPropValue =
                   sun.misc.VM.getSavedProperty("java.lang.Integer.IntegerCache.high");
               if (integerCacheHighPropValue != null) {
                   try {
                       int i = parseInt(integerCacheHighPropValue);
                       i = Math.max(i, 127);
                       // Maximum array size is Integer.MAX_VALUE
                       h = Math.min(i, Integer.MAX_VALUE - (-low) -1);
                   } catch( NumberFormatException nfe) {
                       // If the property cannot be parsed into an int, ignore it.
                   }
               }
               high = h;
   
               cache = new Integer[(high - low) + 1];
               int j = low;
               for(int k = 0; k < cache.length; k++)
                   cache[k] = new Integer(j++);
   
               // range [-128, 127] must be interned (JLS7 5.1.7)
               assert IntegerCache.high >= 127;
           }
   ```

   在启动 jvm 的时候，通过 -XX:AutoBoxCacheMax=<size> 来指定这个缓冲池的大小，该选项在 JVM 初始化的时候会设定一个名为 java.lang.IntegerCache.high 系统属性，然后 IntegerCache 初始化的时候就会读取该系统属性来决定上界。





## 二、String

















































# Java容器及结构实现

### LinkedList

### ArrayList



#### 应用

基于ArrayList实现的LRU算法

1. key-value的对应关系；
2. size值表示当前最新使用的数据；
3. get( )时的倒序处理，保证迭代顺序是“从新到旧”

```java
class LRUCache {

        class Node {

            public int key;
            public int value;

            public Node(int key, int value) {

                this.key = key;
                this.value = value;
            }
        }

        List<Node> LRUDATA;
        final int capacity;
        int size;

        public LRUCache(int capacity) {

            this.LRUDATA = new ArrayList<>(capacity);
            this.capacity = capacity;
            this.size = -1;
        }

        public int get(int key) {
            if (this.size == -1) return -1;
            Node tmp = null;
            Node result = null;
            for (int i = this.size; i >= 0; i--) {
                if ((tmp = LRUDATA.get(i)).key != key) continue;
                LRUDATA.remove(i);
                LRUDATA.add(tmp);
                result = tmp;
                break;
            }
            return result == null ? -1 : result.value;
        }

        public void put(int key, int value) {

            if (get(key) != -1) {
                LRUDATA.remove(this.size);
                Node newNode = new Node(key, value);
                LRUDATA.add(newNode);
                return;
            }

            if (this.size == this.capacity - 1) {

                LRUDATA.remove(0);
                Node newNode = new Node(key, value);
                LRUDATA.add(newNode);
            } else if (this.size < this.capacity) {

                Node newNode = new Node(key, value);
                LRUDATA.add(newNode);
                this.size++;
            }
        }
    }
```









# jvm虚拟机

###- jvm基础

JRE java running environment  

一次编译,多次运行jvm机器; 



混合似的编译器: 

​    即时编译器 和 解释编译器    

​    运行时决定的 “虚方法” 来说,即使编译器更有效率, 

​    不需要多次对虚函数(多态化)解释; 



###- jvm类加载器：

####类加载的三大过程：

1. **加载**：双亲委派模型的加载模式（父类率先加载） - 扩展加载器和应用加载器；

   ​			对java的class文件查找，找到对应字节流，创建对应的类；

   ​			**ps：对与不同的类加载器，加载同一个文件字节流，JVM会识别为不同的类**

2. **链接：**分为下面三大步骤

   1. 验证：
   2. 准备：为静态字段分配内存空间；**同时生成符号引用？**
   3. 解析：将符号引用解析为对应的实际引用（**在这一阶段，如果引用类未加载，将触发类的“1.加载”**）

3. **初始化**: 对类的静态**“基本类型和字符串”**变量（final）（是否和Pool的缓存池有关）；

   ​			  以**🔒**方式执行 类的init( )的方法；

   八大初始化场景：

   1. jvm启动时的函数指定的类
   2. 当new 一个类的时候
   3. 当调用某个类的静态方法时
   4. 当引用某个类的静态变量时
   5. 子类触发父类的初始化
   6. 当通过反射API触发类加载时
   7. 一个接口定义了default方法，直接或间接初始化该类的实现时，会触发
   8. 当调用MethodHandle实例时，初始化，指向的类

启动类加载器(boot class loader)



### 反射的调用

####Inflation机制：

​			前15 次采用代理实现到本地代理实现的方式；之后会通过字节码方式，动态实现。

**ps：-Dsun.reflect.noInflation=true**



关闭后，Method.invoke 将导致**瓶颈**，jvm会记录不同的**动态实现**的调用点（profile）；

执行不同的profile将会导致，Method.invoke被干扰；





### java对象内存

64位虚拟机中：压缩指针   -XX:+UseCompressedOops

**内存对齐？**





#### hashcode和equals

hashcode时Object的区别值，equals的本质是 == 操作，地址相等操作

equals 为 true => hashcode相等 **反之不然**；

String等重写了equals方法，表示值相等。



###Jvm GC机制

可访达分析算法

GC root 简单理解：***堆外指向堆内的引用***

有且不仅限

1. Java 方法栈桢中的局部变量;  ***栈***

2. 已加载类的静态变量; ***非堆***
3. JNI handles; 
4.  已启动且未停止的 Java 线程。 ***栈***



Stop-The-World

safepoint



***JVM参数***

\- XX:+UsePSAdaptiveSurvivorSizePolicy   Eden区的动态分配参数

-XX:SurvivorRatio   Eden区的固定参数比例

-XX:+MaxTenuringThreshold   老年代晋升参数

-XX:+UseCondCardMark   老年代中的卡表减少写回操作

-------







### 小数点保留

1. java.text.DecimalFormat   df   =new   java.text.DecimalFormat("#.00"); 
   df.format(你要格式化的数字);

2. double d = 3.1415926;
   String result = String .format("%.2f");

3. NumberFormat ddf1=NumberFormat.getNumberInstance() ; 

   ddf1.setMaximumFractionDigits(2); 

4. ```java
   static class TT {
   			public static void main(String args[]) {
   				double x = 23.5455;
   				NumberFormat ddf1 = NumberFormat.getNumberInstance();
   
   				ddf1.setMaximumFractionDigits(2);
   				String s = ddf1.format(x);
   				System.out.print(s);
   			}
   		}
   ```

   





### JAVA正则表达式

\\\ - 表示转换符

Pattern.matches（pattern，str）





#Java concurrent相关细节

ThreadPoolExecutor

***runWorker*** 的finally代码块中将执行 ***processWorkerExit***

***tryTerminate*** 方法

中将唤起一个空闲线程进行中断
然后由这个线程来中断其他空闲线程

**递归下去**

```java
    private void processWorkerExit(Worker w, boolean completedAbruptly) {
        if (completedAbruptly) // If abrupt, then workerCount wasn't adjusted
            decrementWorkerCount();

        final ReentrantLock mainLock = this.mainLock;
        mainLock.lock();
        try {
            completedTaskCount += w.completedTasks;
            workers.remove(w);
        } finally {
            mainLock.unlock();
        }

        tryTerminate();

        int c = ctl.get();
        if (runStateLessThan(c, STOP)) {
            if (!completedAbruptly) {
                int min = allowCoreThreadTimeOut ? 0 : corePoolSize;
                if (min == 0 && ! workQueue.isEmpty())
                    min = 1;
                if (workerCountOf(c) >= min)
                    return; // replacement not needed
            }
            addWorker(null, false);
        }
    }
```

 





# Java正则

```java
System.out.println("你".matches("[\\u4E00-\\u9FA5]*"));
System.out.println(("\n").matches("\\n"));
System.out.println(("1").matches("\\d"));

System.out.println(("a12").matches("a\\d"));  //false	-	12算作是两个字符
System.out.println(("a12").matches("a\\d*"));  //true	

System.out.println(("18369185667").matches("1[0-9]{10,10}"));

String regex = "\\b[a|b|c|d|e]\\b";
Pattern pattern = Pattern.compile(regex);
Matcher matcher = pattern.matcher("a b f c d e f");
```

匹配中文字符

```
\\ 转译对应的特殊字符 \\( 表示“左括号”

```

\b 会匹配字符的边界 （空格和字，开始、结尾和字）