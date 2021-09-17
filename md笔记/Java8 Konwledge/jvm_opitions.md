***元数据空间参数：***

**-XX:MetaspaceSize，** 是分配给类元数据空间（以字节计）的初始大小(Oracle逻辑存储上的初始高水位，*the initial high-water-mark* )，此值为估计值。MetaspaceSize的值设置的过大会延长垃圾回收时间。垃圾回收过后，引起下一次垃圾回收的类元数据空间的大小可能会变大。

**-XX:MaxMetaspaceSize，** 是分配给类元数据空间的最大值，超过此值就会触发Full GC，此值默认没有限制，但应取决于系统内存的大小。JVM会动态地改变此值。

**-XX:MinMetaspaceFreeRatio**， 表示一次GC以后，为了避免增加元数据空间的大小，空闲的类元数据的容量的最小比例，不够就会导致垃圾回收。

**-XX:MaxMetaspaceFreeRatio**， 表示一次GC以后，为了避免增加元数据空间的大小，空闲的类元数据的容量的最大比例，不够就会导致垃圾回收。



***分配策略：***

**-XX:PretenureSizeThreshold，**大于此值的对象直接在老年代分配，避免在 Eden 和 Survivor 之间的大量内存复制。

**-XX:MaxTenuringThreshold，** 用来定义年龄的阈值，通过 `MaxTenuringThreshold` 调大对象进入老年代的年龄，让对象在新生代多存活一段时间。



***装箱类型的缓存池参数：***

**-XX:AutoBoxCacheMax=&lt;size&gt;**， 在 jdk 1.8 所有的数值类缓冲池中，Integer 的缓冲池 IntegerCache 很特殊，这个缓冲池的下界是 - 128，上界默认是 127，但是这个上界是可调的，在启动 jvm 的时候，通过 ***-XX:AutoBoxCacheMax=&lt;size&gt;*** 来指定这个缓冲池的大小，该选项在 JVM 初始化的时候会设定一个名为 java.lang.IntegerCache.high 系统属性，然后 IntegerCache 初始化的时候就会读取该系统属性来决定上界。

