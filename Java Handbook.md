# Java
* [类加载过程](#类加载过程)
* [Java内存结构](#Java内存结构)
* [Java垃圾回收器及回收算法](#Java垃圾回收器及回收算法)
* [hashmap和hashtable区别](#hashmap和hashtable区别)
* [JVM调优](#JVM调优)

* [参考](#参考)

------
### 类加载过程
- 加载（类加载器加载到内存）、验证（字节码文件）、准备（分配内存及初值）、解析（符号引用改直接引用）、初始化（类变量和静态代码）
- 双亲委托模型（类加载器（启动类加载器<--扩展类加载器<--应用加载器）和自定义加载器（需委托父加载器加载）），保证安全和避免重复加载
### Java内存结构
- 方法区（默认16M~64M）、程序计数器、堆、虚拟机栈、本地方法栈
### hashcode和equals
- 如果重写 equals() 时没有重写 hashCode() 方法的话就可能会导致 equals 方法判断是相等的两个对象，hashCode 值却不相等
- equals默认和==一样，除非重写
### 异常
- Error和Exception都继承Throwable，包括getMessage()/printTraceStack/toString()方法
- Error程序无法处理，Exception可以处理
- Exception包括检查异常（算术、索引溢出等必须处理）和非检查异常（文件找不到、io溢出）
- 就比如说 finally 之前虚拟机被终止运行的话，finally 中的代码就不会被执行
### 集合
- Map和Collection两大类
- Collection包括Set、Queue、List
- Map包括hashtable、hashmap、treemap（红黑树）
- Set：treeset、hashset
- Queue：priotyqueue、deque
- List：ArrayList、LinkdList
- CopyOnWriteArrayList/ConcurrentHashMap/BlockingQueue(ArrayBlockingQueue或LinkBlockingQueue)并发容器
- JDK1.7 的时候，ConcurrentHashMap 对整个桶数组进行了分割分段(Segment，分段锁)，每一把锁只锁容器其中一部分数据，多线程访问容器里不同数据段的数据，就不会存在锁竞争，提高并发访问率
- JDK1.8 的时候，ConcurrentHashMap 已经摒弃了 Segment 的概念，而是直接用 Node 数组+链表+红黑树的数据结构来实现，并发控制使用 synchronized 和 CAS 来操作。（JDK1.6 以后 synchronized 锁做了很多优化） 整个看起来就像是优化过且线程安全的 HashMap
### Java垃圾回收器及回收算法
- 新生区（串行、串行多线程、并行回收器，复制算法）
- 老年区（串行、串行多线程、CMS，标记-整理或标记-清除）
- G1 复制+标记整理
- 老年区:新生区 = 2：1 Edem : from : to = 8 : 1 : 1
### hashmap和hashtable区别
- hashtable线程安全 hashmap线程不安全 ConcurrentHashMap引入了分段锁更好并且线程安全
- rehash过程要么位置不变要么位置为原来+数组长度
- 每次扩容2倍目的是方便取模和移动，hash值高低位异或+和lenth-1 与操作
- 数组+链表+红黑树实现（当节点大于8个，负载因子0.75）
### String、StringBuilder、StringBuffer区别
- 如果要操作少量的数据用(不可变) = String
- 单线程操作字符串缓冲区 下操作大量数据 = StringBuilder
- 多线程操作字符串缓冲区 下操作大量数据 = StringBuffer

### 并发
- AQS：抽象队列同步，构建同步工具使用，比如Future、Semphone、CountDownLatch、CyclicBarrier等
### Java多态
- 编译时多态：重载函数
- 运行时多态：继承并重写同时父类引用指向子类
### 映射
- 运行时才加载知道名称的类，获取完整构造和对象实体，设置属性和调用方法
### final和static
- final用于类、方法、常量，不能变
- static用于方法和变量，可以变
### 抽象类和接口
- 抽象类有些方法可以默认实现，接口不可以
- 抽象类只能继承一个，接口可以同时实现多个
### 线程池
- Excutors和ExcutorService
- execute()方法用于提交不需要返回值的任务，所以无法判断任务是否被线程池执行成功与否；
- submit()方法用于提交需要返回值的任务。线程池会返回一个 Future 类型的对象，通过这个 Future 对象可以判断任务是否执行成功，并且可以通过 Future 的 get()方法来获取返回值，get()方法会阻塞当前线程直到任务完成
- 7个参数：corePoolSize、maximumPoolSize、workQueue、keepAliveTime、handler、unit、threadFactory
### syncchronized
- 依靠CAS 无锁队列，基本思路时自旋后阻塞，竞争切换后继续竞争
- 原理：synchronized 同步语句块的实现使用的是 monitorenter 和 monitorexit 指令，其中 monitorenter 指令指向同步代码块的开始位置，monitorexit 指令则指明同步代码块的结束位置
- synchronized 修饰的方法并没有 monitorenter 指令和 monitorexit 指令，取得代之的确实是 ACC_SYNCHRONIZED 标识，该标识指明了该方法是一个同步方法
- 不过两者的本质都是对对象监视器 monitor 的获取
### ThreadLocal
- 创建亿ThreadLocal变量时，每个线程都会有一个变量副本
- 实现：每个Thread中都具备一个ThreadLocalMap，而ThreadLocalMap可以存储以ThreadLocal为 key ，Object 对象为 value 的键值对
- 如何解决泄漏问题：ThreadLocal弱引用，可能被回收导致key为null，所以本身会检查key来删除null的项目
### Gc Root
- 虚拟机栈（栈帧中的本地变量表）中引用的对象
- 本地方法栈中 JNI（即一般说的 Native 方法）引用的对象
- 方法区中类静态属性引用的对象
- 方法区中常量引用的对象

### JVM调优
- ```jmap -dump:format=b,file=202208.dump 7132```
- ```java -verbose:gc -XX:+PrintGCDetails -XX:+PrintGCDateStamps -XX:+PrintGCApplicationStoppedTime -Xloggc:groupdv2-jvm.log -XX:PermSize -XX:MaxPermSize -Xmx50m -Xms20m -Done-jar.main.class=com.netease.cc.groupd.Groupd -jar groupdv2-1.0.34-all.jar```
### 参考
- [Java虚拟机](https://bbs.csdn.net/topics/390251794)
- [](JVM参数)

