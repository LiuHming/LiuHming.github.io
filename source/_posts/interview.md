---
title: 面试题
categories:
- 面试
---
# 1.Java面试题

## 1.1Java基础

### ①抽象类与接口区别

* 方法：抽象类可以提供方法的实现，接口中即jdk8以后添加default关键字才可以实现默认实现
* 参数：抽象类成员变量可以任意创建，接口只能是public static final修饰的变量
* 构造函数：接口不能拥有构造器，静态代码块、静态方法，抽象类可以
* 只可继承一个抽象类但是可以实现多个接口

都不可直接实例化是共同点

### ②final、static、synchronized可以修饰什么 什么作用

final:

* 类：说明该类功能已经全面了，不允许继承。String类。Interger
* 方法：该方法不可以被子类重写.final方法编译的时候已经静态绑定了
* 变量：变量不可变  成员变量或是局部变量修饰可以成为最终变量，配合静态static修饰变量称为常量

static：

* 代码块：类进行加载的时候会按顺序执行
* 方法：静态方法属于类的层面，调用的时候用类名.方法名直接调用
* 变量：静态变量，存在Java内存模型中的方法区
* 内部类：可以直接作为一个普通类使用，不需要先实例化一个外部类实例

synchronized：

* 修饰方法：方法为静态方法为类锁，反之为对象锁
* 修饰代码块：锁住的是类为类锁，反之为对象锁

### ③String、StringBuffer、StringBUilder

String为字符串常量，如果要进行拼接+操作，会生成一个新的字符串重新赋予变量，连续拼接操作时会造成一些资源损耗还影响效率

StringBuilder是线程不安全的字符串拼接工具类，append

StringBuffer为线程安全的

### ④equals == hashcode

未重写equals等方法时与==一样，对比引用是否一致

一般equals与hashcode需要统一重写，保证equals为true hashcode必须为true  可反性

### ⑤Error和Exception区别

Error为程序运行时不可预料的错误情况，发生时会直接杀死进程

Exception为运行中可预料到的异常情况，分为检查性异常与非检查型异常，检查性异常需要在编写代码时使用try catch捕获

使用throw抛出的是error

### ⑥什么是反射机制

java反射就是在程序运行过程中通过一些类或方法或是变量的信息对其进行获取操作

获取类信息：

```
obj.getClass()
类名.class
Class.forName("包名+类名")
```

构造方法：

```
Constuctor[] getConstructors()：获取类中所有被public修饰的构造器

Constructor getConstructor(Class...<?> paramTypes)：根据参数类型获取类中某个构造器，该构造器必须被public修饰

Constructor[] getDeclaredConstructors()：获取类中所有构造器

Constructor getDeclaredConstructor(class...<?> paramTypes)：根据参数类型获取对应的构造器

Class clazz = Class.forName("com.bean.SmallPineapple");

Constructor constructor = clazz.getConstructor(String.class, int.class);

constructor.setAccessible(true);

SmallPineapple smallPineapple2 = (SmallPineapple) constructor.newInstance("小菠萝", 21);

smallPineapple2.getInfo();
```

变量：

```
Field[] getFields()：获取类中所有被public、protected修饰的所有变量

Field getField(String name)：根据变量名获取类中的一个变量，获取是被public和protected修饰的

Field[] getDeclaredFields()：获取类中所有的变量，但无法获取继承下来的变量

Field getDeclaredField(String name)：根据姓名获取类中的某个变量，无法获取继承下来的变量
```

方法：

```
Method[] getMethods()：获取类中被public、protected修饰的所有方法

Method getMethod(String name, Class...<?> paramTypes)：根据名字和参数类型获取对应方法，该方法必须被public、protected修饰

Method[] getDeclaredMethods()：获取所有方法，但无法获取继承下来的方法

Method getDeclaredMethod(String name, Class...<?> paramTypes)：根据名字和参数类型获取对应方法，无法获取继承下来的方法
```

注解：只有@Retension为RUNTIME时才能通过反射获取该注解

```
Annotation[] getAnnotations()：获取该对象上的所有注解

Annotation getAnnotation(Class annotaionClass)：传入注解类型，获取该对象上的特定一个注解

Annotation[] getDeclaredAnnotations()：获取该对象上的显式标注的所有注解，无法获取继承下来的注解

Annotation getDeclaredAnnotation(Class annotationClass)：根据注解类型，获取该对象上的特定一个注解，无法获取继承下来的注解
```

### ⑦IO流分几种

IO流分为字节流和字符流，与之对应的抽象类有inputstream、outputstream、reader、writer

字节流可以传递存储任何类型的数据，字符流只能处理字符、字符串，使用指定的字符集进行编码操作

BIO：同步阻塞式IO  NIO：同步非阻塞 AIO：NIO升级也叫NIO2，异步非阻塞

### ⑧泛型类型擦除

类型擦除发生在编译过程中，所有泛型信息都会被擦除

### ⑨注解的理解

本质：做一个标识，通过这个标识对代码规范、变量值做一些修饰。主要划分为三类


| Source  | 仅仅存在在.java文件，编译成.class文件就消失了。作用为：让开发者按照注解的规范编写代码。例如：[@OverRide](https://github.com/OverRide)                        |                                                        |
| --------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------- |
| Class   | 在前期编译期的流程中，会被处理成.class内容，与原生代码效率几乎相同。<br />作用为：自动生成.class文件，做一些辅助性工作。例如：ButterKnife、GreenDao、ARouter | 效率和原生代码相当                                     |
| Runtime | 编译成.class文件之后，依旧以注解的方式存在。而是在运行期生效。<br />作用为：在运行期，通过反射做一些辅助性工作。例如：xUtils                                 | 由于集中使用遍历+反射，因此效率较低。而且在9.0禁用反射 |

## 1.2 Java集合

### ①list、set、map

Iterator是所有集合的总接口，Collection继承于Iterator。list有序可重复，set无序不可重复，map键值对，键唯一。

list、set继承自collection。ArrayList数组结构，存储地址连续、linkedList双向链表结构。

hashset哈希表实现，无序可放入null

treeset是二叉树红黑树结构实现的，自动排序不允许null值

hashmap hashtable concurrentHashMap、linkedhashmap

## 1.3 Java多线程

### ① java 多线程的方式

```
1. 继承Thread，重写run方法，用start启动
2. 自定义runnable，放入thread启动
3.通过线程池、callable、future实现有返回结果的多线程
```

### ② 线程的状态

* new 创建状态
* runnable 运行
* blocked 阻塞
* waiting 等待
* timed_waiting 超时等待
* end 终止

### ③ 实现多线程中的同步

* volatile 简单逻辑可以实现 简单的 set get
* synchronized
* reentrantlock

  ```
  1. Lock lock = new ReentrantLock();
     try {
              todo.........
          } finally {
              lock.unlock();
          }
  也可以尝试获取锁
  if (lock.tryLock(1, TimeUnit.SECONDS)) {
      try {
          ...
      } finally {
          lock.unlock();
      }
  }
  2.配合condition的await和signal模仿obj的wait、notify
  3.  ReadWriteLock rwlock = new ReentrantReadWriteLock();
      Lock rlock = rwlock.readLock();
      Lock wlock = rwlock.writeLock();
  ```
* cas   unsafe类
* Object.wait/notify

  ```
  wait属于object类中的方法，调用后释放锁，将当前线程a放入该对象锁的等待池，线程b执行notify会通知等待池，使线程a从等待池进入阻塞队列等到线程b释放锁后线程a竞争锁继续执行。
  notifyall会把等待池里所有的线程唤醒去竞争
  ```

### ④ 线程池

* 常见线程池

  ```
  Executors.newFixedThreadPool 使用的是linkedblockingqueue  指定核心线程数
  Executors.newCachedThreadPool 使用的是SynchronousQueue
  Executors.newSingleThreadExecutor 使用的是LinkedBlokingQueue
  Executors.newScheduledThreadPool

  ThreadPoolExecutor  核心线程数 最大线程数 超时回收时间 时间单位 任务处理队列 线程工厂
  ```

  核心线程-》阻塞队列-》非核心线程-》handler拒绝任务提交
* 常见的任务处理阻塞队列
  ArrayBlockingQueue 有界数组阻塞队列
  LinkedBlockingQueue 无界链表阻塞队列
  SynchronousQueue 无存储队列
  PriorityBlockingQueue 优先级阻塞队列
* shutdown 关闭线程池——等添加到线程池中的任务全部完成才退出
  shutdownNow  关闭线程池并中断任务  调用Tread.interrupt中断任务 但线程无sleep、wait、condition、定时锁等是无法中断新城，有可能需要等所有线程执行完毕关闭

### ⑤ synchronized与volatile

volatile：

1. 保证变量在不同线程进行操作的可见性
2. 禁止指令重排
   指令重排是指指令乱序执行，有时在条件允许情况下，为了避开获取一条指令所需的数据而造成的等待，会通过乱序执行提高效率。添加一个内存屏障，指令乱序时不能把后面的指令排序移到内存屏障之前

synchronize：

偏向锁、轻量级锁。自旋锁、重量级锁

### ⑥ 死锁

当线程a持有独占锁1，并尝试去获取独占锁2的同时，线程b持有独占锁2，并尝试获取独占锁1，这时两个线程都出去获取对方的独占锁处于阻塞状态，产生了死锁。

1. 互斥条件：一个资源只能被一个线程使用 独占锁
2. 请求与保持条件：一个线程因请求资源被阻塞，对已获得的资源保持不释放
3. 不剥夺条件： 线程获得资源在未使用完之前，不能强行剥夺
4. 循环等待：若干线程之间形成头尾相接的循环资源等待关系

解决办法：

1. 加锁顺序控制 按照一定顺序加锁
2. 加锁时限 线索尝试获取锁时加上时限，超时则放弃对锁的请求，并释放自己占有的锁
3. 死锁检测 加锁时将线程与锁存在类似map中，每当有线程请求锁失败时可以判断是否有循环锁占有和请求的关系进行处理

### ⑦ 线程、进程

一个程序至少有一个进程，一个进程至少有一个线程

1. 进程是资源分配最小单位，线城市程序执行的最小单位。
2. 进程有自己独立的地址空间，每启动一个进程系统会为其分配地址空间。线程没有独立的地址空间，统一进程的线程共享进程的地址空间
3. 进程之间资源独立，同一进程内线程共享本晋城资源
4. 每个独立进程有一个运行入口，顺序执行队列和程序出口。线程不能独立执行，依存于程序进程中

### ⑧ ThreadLocal

1. 原理：
   每一个tread内部维护了一个threadlocalmap，内部维护了一个数组，数组里面存储的数据是一个entry内部类，key为threadlocal的弱引用，value为我们要存储的数据的强引用。
   set方法：从当前线程获取他的localthreadmap，根据hashcode与数组长度计算在数组中存储的位置，分为几个情况
   如果以前在当前位置存储过，更新后返回，发生hash冲突依次向后寻找。返回
   如果当前key为null，也就是被回收了，会先向前遍历直到entry为空，找到第一个该回收的位置，然后再向后遍历查找是否和当前key匹配的有的话进行位置交换及清理,返回
   如果没有存储过就直接新建存储，进行清理并检查需不需要扩容；
   在执行get操作时，从当前线程获取他的localthreadmap，根据threadlocal的hashcode与数组长度计算存储的数组位置，通过当前的threadlocal获取我们存储的值。
2. 内存泄漏：因为value为强引用，threadlocal回收后未去回收他的value就会发生内存泄漏，不用时应该保证执行remove或保证与线程生命周期相同

## 1.4 Java内存

### ① 类的加载

* android中有5个类加载器：ClassLoader（所有类加载器的抽象基类）、BootClassLoader（用于加在android系统的类，classloader的内部类，开发者无法调用）、BaseDexClassLoader（继承ClassLoader）、PathClassLoader（继承于BaseDexClassLoader，通常用于加载我们自己写的类含第三方库，但不局限于此）、DexClassLoader（继承于BaseDexClassLoader，通常用于执行动态加载，能够加在指定路径的apk、jar、zip、dex文件，因此很多热修复和插件化方案使用）
* 加载时使用双亲委派模式加载，先以递归方式向上级父加载器验证是否已经加载，若没被加载过在从最顶级加载器进行加载操作，加载失败逐级向下进行加载。保证类只会被加载一次。
* 加载（通过类的全限定名获取类的二进制字节流，将静态存储结构转化为方法区的运行数据结构，在内存中生成Class对象，作为这个类的各种数据的方位入口）-》
  连接（1.验证——语法是否通过；2.准备——变量分配内存；3.解析——接口、字段和方法的符号引用转为直接引用）-》
  初始化（对变量和一些代码块进行初始化及执行）

### ② 引用类型

1. 强引用：垃圾回收器不会回收，及时内存空间不足，jvm宁愿outofmemoryerror程序异常终止也不随意回收强引用对象
2. 软引用：如果一个对象只有软引用，内存够用的时候不会对其进行回收，当内存不足时hiuduiqijinxinghuishou，可以和引用队列联合使用
3. 弱引用：如果一个对象只有弱引用，垃圾回收器检测到就会对其进行回收。由于垃圾回收器所在线程优先级很低不一定会快发现，可以和引用队列联合使用
4. 虚引用：任何时候都可能被回收，主要用于跟踪对象呗垃圾回收器回收的活动。必须与ReferenceQueue联合使用

### ③ Java内存模型

1. 堆  对象
2. 栈  线程栈帧 线程私有
3. 本地方发栈
4. 方法区 加载的类信息、常量、静态变量、即时编译器编译后的代码
5. 程序计数器

### ④ Java内存回收机制

* 回收检测有两种：
  引用计数法：有对这个对象的引用+1，不再引用-1
  可达性分析：以GCRoots对象为起点，从这个节点从上到下搜索，路径称为引用链，当一个对象没有任何引用链与GCRoots连接时，就说明当前对象不可达可以回收
  GC Roots对象通常包括：

  ```
  虚拟机栈中引用的对象——栈帧中的本地变量表
  方法中类的静态属性引用的对象
  方法区常量引用的对象
  Native方法引用的对象
  ```
* 回收算法：

  ```
  1. 标记清除：
     扫描并标记存活目标，标记完成后再扫描未标记的对象对其进行回收。存活对象多时效率高
  但是会造成内存碎片。
  2. 标记整理：
     在标记清除基础上，将存活的内存进行移动整理更新指针。适合对象存活率高的场景——老
  年代回收。
  3. 复制：
     将内存分为大小相等两块，每次只是用一块。当内存使用完后，将存活对象复制到另一侧，
  再将该区域全部清除。适用于对象存活率低情况——新生代。
  4. 分代收集：
     不同的对象生命周期不同，不同的生命周期的对象放置在堆中的不同区域，以不同的策略
  进行回收提高效率。新生代使用复制算法；老年代使用标记清除或标记整理。分代收集一般分
  为新生代，老年代和永久代。
     新生代：所有新对象首先都放在新生代，按照8：1：1比例分为一个Eden区和两个survivor区。
  大部分对象在Eden区生成，回收时先将Eden存活对象复制到survivor0然后清空Eden，
  当这survivor0存放满，将Eden区域survivor0中存活的对象复制到survivor1中，
  清空Eden与survivor0，然后将survivor0与survivor1互换保证survivor1为空。
  当survivor1不足存放时就会将存活对象存放至老年代。若是老年代满了就会出发full GC，
  也就是新老年代都进行回收。新生代的GC叫做MinorGC发生频率高。
     老年代： 内存大概是新生代的两倍，老年代中的对象存活时间长。
     永久代： 主要存放静态文件，如java类、方法等。对垃圾回收没有显著影响，用来解决
  动态代理或是反射等等运行中新增的类的内存。

  ```

### ⑤ jvm、dalvik和art

* jvm：java虚拟机，并不是某个特定虚拟机的实现，而是任何能运行java字节码的虚拟机实现
* dalvik：是google创建的用于android的虚拟机，但其严格来说不算jvm，5.0被art替代
  基于寄存器，jvm基于堆栈；有自己的字节码不是java字节码；jit即时编译
* art：android run time。4.4-6.0采用安装时全部编译为机器码的方式实现，7.0开始默认不全部编译，采用解释执行+jit+空闲时间aot以改善安装耗时。
  aot预编译在安装过程中，将所有字节码变异成机器码，运行时直接调用。

# 2 Kotlin

## 2.1 kotlin语言特性

### ① 介绍kotlin其特性

* 能与java互相调用
* 减少样板代码
* 可将kotlin编译为无需虚拟机就可以运行的原生二进制文件
* 支持高阶函数
* 支持协程
* 语言层面解决空指针问题
* 对lambda表达式更好地支持

### ② @JvmOverloads作用

在有默认参数值的方法中使用@JvmOverloads注解会重载多个方法。

### ③ kotlin中单例与java对比

1. 线程不安全饿汉式

   ```
   kotlin：
   object Single{
   }

   java：
   class Single {
       private static Single instance = new Single();

       public static Single getInstance(){
           return instance;
       }
   }
   ```
2. 线程不安全懒汉式

   ```
   kotlin：
   class Single{
       companion object{
           val instance by lazy { Single() }
       }
   }

   java：
   class Single {
       private static Single instance;

       public static Single getInstance(){
           if (instance == null){
               instance = new Single();
           }
           return instance;
       }
   }
   ```
3. 双重校验

   ```
   kotlin：
   class Single{
       companion object{
           val instance by lazy(LazyThreadSafetyMode.SYNCHRONIZED) { Single() }
       }
   }

   java:
   class Single {
       private volatile static Single instance;

       public static Single getInstance(){
           if (instance == null){
               synchronized (Single.class){
                   if (instance == null){
                       instance = new Single();
                   }
               }
           }
           return instance;
       }
   }
   ```
4. 静态内部类

   ```
   kotlin：
   class Single{
       companion object{
           val instance = SingleHolder.Single
       }
       private object SingleHolder{
           val Single = Single()
       }
   }

   java：
   class Single {
       private static class SingleHolder{
           public static final Single holder = new Single();
       }

       public static Single getInstance(){
           return SingleHolder.holder;
       }
   }

   ```

### ④ dataClass

会自动生成equals/hashCode方法、toString、componentN解构方法和copy方法

### ⑤ with、run、also、apply、let

* let、also、apply、let都是作为扩展函数，传入一个lambda表达式
  let：返回值是最后一行或return的数据
  also： 返回的是对象本身，this表示可省略
  run：返回最后一行的值或表达式
  apply：返回本身
  let与also会将属性作为参数传入lambda中，所以使用属性需要用it表示，let返回lambda执行结果，also返回属性值
  run与apply传入的lambda是带属性T的接受者对象的函数，run返回的是执行结果，apply是返回属性this
* with 是传入两个参数，第一个是属性值，第二个是属性的扩展lambda函数并返回

### ⑥ Unit与Void区别

Unit：Kotlin中Any的子类，作为返回类型时可省略

Void：java中无返回类型时使用，关键字，不可行略

nothing：一般形容无法继续执行，常常和抛出异常一起使用

### ⑦ 关键修饰符

1. 中缀函数 infix 定义的函数必须只有一个参数，可省略.与参数括号
2. 密封类 sealed 表示受限的类继承结构，本身是抽象类不可以实例化，必须和子类在同一个包中
3. init函数执行顺序： 主构造函数 > init > 次级构造函数
4. object 与 companion object 的区别** object 有两层语义：静态匿名内部类 + 单例对象 companion object 是伴生对象，一个类只能有一个，代表了类的静态成员（函数 / 属性）
5. inner 非静态内部类， kotlin内部类默认是静态内部类，如果要使用类中成员方法和属性需要加inner
6. inline 内联函数，表示将函数内容。如果我们是用lambda表达式作为参数，lambda在编译后转换为匿名类或是静态对象会造成额外开销。
   noinline 当函数被inline标记，参数使用该标记不使用内联，不可return主函数
   crossinline 允许内联函数里的函数类型参数可以被间接调用，但是不能在Lambda表达式中使用全局return返回

### ⑧ any与object区别

* 相同：都是顶级父类
* 差异：any只声明toString/equeals/hashcode作为成员方法,没有wait、notify、clone、finalize等，属于兼容java平台的平台类型。

### ⑨ 委托

* Observable

  ```
  var name: String by Delegates.observable(初始值){prop,old,new ->
  }
  ```
* 自定义委托 ReadOnlyProperty、ReadWriteProperty、PropertyDelegateProvider
* 懒加载委托

## 2.2 协程

### ① 协程的使用

* 协程通过CoroutineScope创建，启动方式有三种：1.runBlocking——启动一个新协程并阻塞调用他的线程，直到里面的代码执行完毕，返回最后一行；2.launch——启动一个协程不会阻塞调用线程，必须在协程作用域中才能调用，返回job；3.async——启动一个协程不会阻塞调用线程，必须在协程作用域中才能调用。返回Deferred（继承job）。
* GlobalScope全局顶级协程，这个协程是在整个应用程序生命周期内运行的。使用launch、async。
* suspend是协程关键字，代表挂起函数，只能在suspend方法或者在协程中调用。

# 3 android

## 3.1 各框架原理

### ① LeakCanary

目前最新版本仅用debugImplementation就可以实现内存泄漏监听。这是利用了ContentProVider进行初始化，在应用启动时回调他的oncreate方法。在oncreate方法里面进行他的多种watcher的安装。内部创建对于activity、fragmentAndViewModel、rootView、service的watcher，并依次进行install操作。

ContentProVider的oncreate在于application的oncreate（handleBindApplication），

晚于application的attachBaseContext。

先进性对应该回收对象的生命周期加入钩子。

1. ActivityWatcher会在install操作中通过by noOpDelegate委托生成动态代理 application.registerActivityLifecycleCallbacks对其进行onActivityDestroyed监听，当activity调用ondestroyed时会进行内部ObjectWatcher的expectWeaklyReachable方法，检查五秒后是否被回收，未被回收则标记内存泄漏。
2. FragmentAndViewModelWatcher在开始与ActivityWatcher一样，不过是注册activity的onactivityCreated监听。在oncreate的时候对他的fragmentwatcher数组依次进行。
3. RootViewWatcher是使用Curtains的onRootViewsChangedListeners，在detached的时候验证
4. ServiceWatcher是通过反射activityThread拿到mservices，对activityThread中的handler的mcallback进行动态代理，在service真正destroy的时候进行验证

ObjectWatcher进行对象的是否内存泄漏检查：

1. 先移除已被回收的对象
2. 用UUID生成一个key，然后同观察对象、描述、引用队列等创建弱引用。并根据key将reference存入map里。
3. 在默认5s延迟后再清理一遍，检测对象是否被回收
4. 没有回收调用onObjectRetainedListener

objectRetainedListener里面主要执行heapDumpTrigger的一些方法

1. 首先判断是否有异常持有的对象，有的话触发Gc操作，没有就返回
2. 如果还有异常判断泄露数量是否达到dump
3. 达到的话会进行dumpHeap
4. 然后使用HeapAnalyzerService，启动shark分析
5. 读取hprof内存快照文件
6. 找到leakCanary标记的泄露对象的数量和弱引用包装的ids
7. 找到gcRoot开始的路径

### ② Eventbus

EventBus.getdefault方法其实是一个双重校验的单例方法，内部涉及几个map

首先是一个method_Cache的concurrentHashmap，他的key是类信息，value是解析过的方法数组

subscriptionsByEventType，key是event的类信息，value是存储subscription的copyOnWriteArrayList

在我们register的时候。

1. 首先获取注册类信息，并对其进行方法的提取
2. 在method＿cache查找是否有解析过的缓存
3. 没有的话，先创建一个findState维护类的相关数据。
4. 通过反射拿到类里面的所有方法，通过方法的修饰符（必须public，非static，非abstract）和参数数目进行过滤，再将过滤出的方法进行自己的注解比对
5. 完全符合会根据注解获取threadmode等等和其他信息整理成一个subscibermethod，存放在findstate的监听方法数组里
6. 在检查父类有没有相应的方法需要添加，当获取所有方法完毕后，对这些方法依次进行subscriber操作
7. 将注册对象与subscibermethod封装成一个subscription，然后再存在subscriptionsByEventType的当前eventType的数组里
8.

在我们post事件时

1. 将事件存放在threadlocal里的postingState的eventQueue里
2. 取出第一个event，获取他的类信息，在eventTypesCache里面找有没有event数组，里面是当前事件及其基类的class对象和接口基类的class对象
3. 然后对这些事件进行分发处理，在subscriptionsByEventType找到对应的subscription数组。
4. 将每一个subscription与当前事件、当前线程是否主线程共同传入postToSubscription方法中
5. 通过当前线程是否是主线程和subscription中的threadMode进行判断，决定是否切换线程，不需要切换直接反射调用。
6. 需要切换线程时，再将subscription与event封装一起发送到指定线程执行。

### ③ retrofit

1. 首先在retrofit记录下配置的calladapterFactories和converterFactories
2. 在执行retrofit.create的时候，先验证apiService的合法性，再进行动态代理实现具体方法。
3. object或是platform的方法直接执行
4. RequestFactory的parseAnnotations方法解析method中的注解
5. 然后传递给HttpServiceMethod创建OKhttpCall并进行adapt操作
6. 根据我们的callAdapterFactory包装成我们指定的call对象，默认是retrofitCall
7. 创建request对象使用okhttp进行请求，拿到结果后通过配置的converterFactory解析成我们指定的数据格式

### ④ okhttp

* 大概流程：

1. 通过建造者模式构建okhttp和request
2. okhttpClient通过newCall发起请求
3. 通过分发器维护请求队列和线程池，完成接口调配
4. 通过五大默认拦截器完成请求重试，缓存处理，建立连接等一些列操作
5. 得到结果完成回调

* okhttp分发器工作流程：

分发器主要维护请求队列与线程池

同步任务请求不需要线程池，只需按照加入队列的顺序请求

异步请求则要进行判断正在执行的任务未超过最大限制64，同时同一host的请求不超过5个，添加在正在执行队列，同时提交给线程池。否则加入到等待队列。每个任务完成后都会调用分发器的finished方法，在等待队列中取出等待队列的任务继续执行。

* 拦截器

执行getResponseWithInterceptorChain方法是使用了责任链模式，依次进行重试与重定向拦截器、桥接拦截器、缓存拦截器、连接拦截器、自定义拦截器和请求拦截器。分别负责重试重定向、处理header、缓存、连接池等

### ⑤ Android与js交互

js调用Android

* 创建一个处理交互的方法类，在方法上面添加@JavascriptInterface，再执行webview.addJavascriptinterface()进行映射。
* 通过 `WebViewClient`的shouldoverrideUrl()进行链接拦截处理
* 通过 `WebChromeClient`的onjsaler confirm prompt

android 调用 js

* webview.loadurl（“`javascript:`”）
* `WebView.evaluateJavascript(`)

webview优化

1.独立进程，android：process 退出system。exit

2.预加载 建立webview维护类。使用mutablecontextwrapper 优先传入applicationcontext，后面可以通过重新设置basecontext更改绑定特定的activity

### ⑥ 其他

多渠道包：productflavor配置不同applicationid，将公有库统一维护，各自业务使用风味名+implement进行引用，一些第三方库的一些秘钥使用manifestplaceholder进行维护

intentservice是继承service来创建工作线程，内部原理是在oncreate的时候创建一个handlerthread和他的servicehandler，在onstartcommand里面会点用onstart进行message的发送，在连续进行任务时可以按顺序进行处理。在任务都处理完成之后会自动结束。
