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

### syncchronized
- 依靠CAS 无锁队列，基本思路时自旋后阻塞，竞争切换后继续竞争
### ThreadLocal
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

