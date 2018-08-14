---
title: JVM内存管理
date: 2018-08-06 17:08:32
categories:
- Java虚拟机
tags:
- Java
- JVM
---

  ![](http://pbsg2r9io.bkt.clouddn.com/18-8-6/89580607.jpg)
  
  本文主要目的是从学习**深入理解Java虚拟机**中，针对性的做下读书笔记。整理主要从以下几个方面来梳理 JVM的内存区域划分、JVM的内存溢出、JVM的GC算法、JVM的类加载机制等。
  
  <!-- more -->
  
### JVM的内存区域划分

  ![](http://pbsg2r9io.bkt.clouddn.com/18-8-6/99649423.jpg)
  
  Java堆和方法区是线程共享的数据区，虚拟机栈、本地方法栈和程序计数器则是由线程独有的数据区。
  
  + **程序计数器**：一块较小的内存空间，可以看做当前线程执行字节码的行号指示器。
    + 虚拟机多线程是由线程轮流切换分配CPU的执行权限，为了保证线程切换后能恢复到正确的执行位置，每条线程需要一个独立的程序计数器。 
  + **方法区**：存放加载的类信息和运行时的常量池信息(字符串和数字常量)
  + **Java堆**：虚拟机启动时候创建，是Java程序最主要的内存工作区域。所有的Java对象实例及数组都放在Java堆中。所有线程共享堆内存。
    + 随着JIT编译器的发展和逃逸分析技术的成熟，所有对象分配在堆上的说法也不再绝对。
  + **虚拟机栈**：每个线程私有。保存局部变量、方法参数等。
  + **本地方法栈**：与虚拟机栈类似，供本地方法调用。

#### 运行时常量池    

### 对象的创建
  
  `Person p = new Person()`
  
  对象的创建是由一个new指令开始的，程序new一个对象的时候，虚拟机首先检查指令的参数是否在常量池(方法区)定位到一个类引用，并且该类是否已记载、解析、初始化。(如果没有，需要先执行相应的类加载 Chapter7)。
  
  类加载检查完成后，虚拟机为对象分配内存。对象所需内存的大小在类加载完成后可以确定(Chapter2.3.2)。分配方式有指针碰撞与空闲列表两种。
  
  内存分配完成以后，虚拟机需要将分配到的内存空间都初始化为零值。这一步操作保证了对象的实例字段在Java代码中可以不赋初始值就直接使用，程序能访问到这些字段的数据类型所对应的零值。
  
  接下来，设置对象的对象头信息（对象是哪个类的实例、如何才能找到类的元数据信息、对象的哈希码、对象的GC分代年龄）
  
  至此，虚拟机的对象创建已经完成。剩下的就是执行对象init方法进行对象的初始化了。
  
#### 虚拟机分配内存方式

  + 指针碰撞(Bump the Pointer): 如果堆内存是规整的，空闲内存一边，已使用内存一边，中间一个指针作为分界点。则分配内存仅仅是把指针向空闲空间挪动一段与对象大小相等的距离。
  + 空闲列表(Free List): 如果堆内存是不规整的，就没办法进行简单的指针碰撞了。虚拟机需要维护一个列表记录内存是空闲还是已使用的。分配的时候找到一块足够大的空间划分给对象实例。
  
  具体选择哪种分配方式由堆内存是否规整决定，堆内存是否规整则由所使用的的垃圾收集器是否带有压缩整理功能决定。这里使用Serial、ParNew等带Compact过程的收集器时，系统采用指针碰撞方式分配，
  而使用CMS的基于Mark-Sweep算法收集器通常采用空闲列表。
  
#### HotSpot的GC收集器
  
#### 分配内存在并发场景下如何保证线程安全
  
  对象的创建是个很频繁的操作，即使一个简单的指针位置变动，在并发场景下，可能正在给A对象分配内存，指针没来得及修改，对象B也同时使用同一个指针分配内存。
  
  + CAS失败重试。虚拟机实际上以该方式确保指针更新操作的原子性。
  + 本地线程分配缓冲(Thread Local Allocation Buffer),每个线程在Java堆中预先分配一小块内存，把内存分配的动作按线程划分在不同的空间进行。可以通过-XX：+/-UseTLAB参数来设定。
  
#### 栈上分配
  
### 内存溢出

#### Java堆内存溢出  
  
  Java堆存储对象实例，循环创建对象，并确保GC Roots到对象之间有可达路径，以此避免垃圾回收机制清除这些对象。那么当对象容量达到最大堆容量限制，则产生内存溢出异常。
  
  -Xms 设置最小堆内存，-Xmx设置最大堆内存
  
  ```java
  /**
   * args: -Xms20m -Xmx20m -XX: + HeapDumpOnOutOfMemoryError
   * 堆的内存溢出示例
   */
  public class HeapOOM {
  
    public static void main(String[] args) {
      List<HeapOOM> list = new ArrayList<>();
      while(true){
        list.add(new HeapOOM());
      }
    }
  }
  
//  Exception in thread "main" java.lang.OutOfMemoryError: Java heap space
  ```
    
#### 虚拟机栈与本地方法栈溢出
  
  线程请求的栈深度大于虚拟机所允许最大深度时，将抛出StackOverflowError异常。
  
  虚拟机在扩展栈时无法申请到足够内存空间时，将抛出OOM异常。
  
  HotSpot虚拟机不区分虚拟机栈和本地方法栈。故-Xoss基本无用。-Xss参数设置栈容量。
  
  ```java
  /**
   * -Xss128k
   */
  public class VmStackSOF {
  
    private int stackLen = 1;
  
    public void stackLeak() {
      stackLen++;
      stackLeak();
    }
  
    public static void main(String[] args) {
      VmStackSOF oom = new VmStackSOF();
      try {
        oom.stackLeak();
      } catch (Throwable e) {
        System.out.println("stack length : " + oom.stackLen);
        throw e;
      }
    }
  }
  // Exception in thread "main" java.lang.StackOverflowError
  // 	at com.mk.jvm.VmStackSOF.stackLeak(VmStackSOF.java:12)
  ```
  
  ```java
  public class VmStackSOF {
    private void dontStop() {
      while (true) {}
    }
    public void stackLeakByThread() {
      while (true) {
        Thread t = new Thread(() -> {dontStop();});
        t.start();
      }
    }
    public static void main(String[] args) throws Throwable {
      VmStackSOF oom = new VmStackSOF();
      oom.stackLeakByThread();
    }
  }
  // Java线程映射到Windows内核线程，所以这个会导致系统假死
  ```
#### 方法区和运行时常量池内存溢出
  
  运行时常量池是方法区的一部分(JDK1.6)。
  
  ```java
  /**
   * JDK1.6 Vm Args: -XX:PermSize=10M -XX: MaxPermSize=10M
   * JDK1.7 Vm Args: -Xmx20m -Xms20m -XX:-UseGCOverheadLimit
  public class RunTimeConstantsPool2 {
    public static void main(String[] args) {
      List<String> list = new ArrayList<String>();
      int a = 0;
      while (true) {
        list.add(String.valueOf(a++).intern());
      }
    }
  }
  // JDK1.6:  java.lang.OutOfMemoryError: PermGen space
  // JDK1.7:  java.lang.OutOfMemoryError: Java heap space
  ```
  
  从异常来看，1.6的运行时常量池是在方法区中，1.7则已经被移到堆中。


#### 知识点扩展
  + 如何排查内存溢出。      
  + 关于运行时常量池1.6版本与1.7及之后版本的区别。
  + string.intern()方法的区别

#### 小结  

  内存溢出主要有：堆溢出(OOM: Java Heap Space)、方法区溢出(OOM: PermGen Space)、虚拟机栈溢出(SOF)

### JVM的垃圾回收

  JVM内存区域划分中，程序计数器、虚拟机栈与本地方法栈的内存当类结构确定后所占内存就已经确定，并且他们都是线程独有。当方法或线程结束后，内存自然回收。
  
  所以我们这里讨论的回收主要是堆、方法区。

#### 对象是否存活

##### 引用计数算法

  给对象添加一个引用计数器，有一个引用时，加1；引用失效时，计数器减1。计数器为0的对象即为不再被使用。
  
  优点：简单、效率高。
  
  缺点：很难解决对象间的循环引用。 (Java主流虚拟机中未使用该算法) 
  
  ```java
  // VmArgs : -XX:+PrintGC XX:+PrintGCDetails  -XX:+PrintGCTimeStamps
  public class ReferenceCountingGC {
  
    public Object instance = null;
    private final static int _1MB = 1024 * 1024;
  
    private byte[] bigSize = new byte[2 * _1MB];
  
    public static void testGC() {
      ReferenceCountingGC objA = new ReferenceCountingGC();
      ReferenceCountingGC objB = new ReferenceCountingGC();
      objA.instance = objB;
      objB.instance = objA;
      objA = null;
      objB = null;
  
      System.gc();
    }
  }
  // GC日志显示虚拟机回收了objA/objB。即使他们相互引用。侧面说明虚拟机使用的不是引用计数算法。
  ```

##### 可达性算法

  通过一系列称为"GC Roots"的对象作为起始点，从GC Roots节点向下搜索，搜索的路径称为引用链(Reference Chain), 当某个对象到GC Roots没有任何引用链相连，则该对象不可用。
  
  + Java中，可作为GC Roots对象的包括以下几种：
    + 虚拟机栈(栈帧中的本地变量表)中引用的对象。
    + 方法区中类静态属性引用的对象。
    + 方法区中常量引用的对象。
    + 本地方法栈中JNI(Native方法)引用的对象。

##### 不可达后是否死亡

  在可达性算法分析中不可达的对象，暂时还没有死亡。死亡之前还有两次标记过程。
   
  对象不可达时，虚拟机会对该对象第一次打上标记
    判断对象是否重写finalize方法，如果重写则将其放入一个F-Queue,稍后会有一个Finalizer线程执行它(对象在这里可以逃脱)。
    如果没有重写或方法已执行，则被过滤。
     
  *Finalizer线程不保证执行finalize方法后会等待它执行结束*   
   
  ```java
  public class FinalizeEscapeGC {
    public static FinalizeEscapeGC SAVE_HOOK = null;
    public void isAlive(){
      System.out.println("yes , i am still alive.");
    }
    @Override
    protected void finalize() throws Throwable {
      super.finalize();
      System.out.println("finalize method executed");
      FinalizeEscapeGC.SAVE_HOOK = this;
    }
    private static void goDead() throws InterruptedException {
      SAVE_HOOK = null;
      System.gc();
      // 因为finalize方法优先级低，暂停0.5秒等待
      sleep(500);
      if (SAVE_HOOK != null) {
        SAVE_HOOK.isAlive();
      }else{
        System.out.println("no, i am dead.");
      }
    }
    public static void main(String[] args) throws InterruptedException {
      SAVE_HOOK = new FinalizeEscapeGC();
      // 对象第一次拯救自己
      goDead();
      // 第二次对象死掉
      goDead();
    }
  }
  /*
   * finalize method executed
   * yes , i am still alive.
   * no, i am dead.
   */   
  ```
  
#### 垃圾收集算法

##### 标记-清除算法

  最基础的垃圾收集算法，后续的所有收集算法都是基于这种方法的改进。
  
  标记所有需要回收的对象，标记完以后统一回收。（标记过程见4.1）
  
  缺点：
    
   1. 效率低
   2. 空间问题。标记-清除会产生大量的不连续内存碎片，当程序后续需要分配较大对象时，可能会因无法找到足够的连续内存提前触发GC。   
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-8-8/80407106.jpg)

##### 复制算法

  将内存等分A/B两份， 触发GC时，将A中存活的对象复制到B，然后将A全部清理掉。
  
  优点：简单、高效
  
  缺点：内存空间变为一半，代价太高。
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-8-8/45488501.jpg)
  
  目前，商业虚拟机回收新生代使用的该算法。HotSpot虚拟机默认Eden:Survivor=8:1, 这里内存划分了一块Eden空间和两块Survivor空间。
  
  所以每次新生代可用内存为整个新生代容量的90%。只有10%的空间浪费。
  
  （如此划分是因为研究表明新生代对象98%是可回收的，所以每次GC会将Eden和Survivor的对象复制到10%的另一块Survivor中）
  
##### 标记-整理算法

  根据老年代的特点，提出的标记-整理算法。
  
  标记过程与标记-清除算法类似，后续则是将存活对象往一端移动，GC时直接清理掉端边界以外的内存。
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-8-8/95829161.jpg)

##### 分代收集算法

  当前商业虚拟机都是采用分代收集算法(Generational Collection)，不同区域选择不同的算法。
  
  Java堆-新生代：对象存活率低，使用复制算法。
  Java堆-老年代：对象存活率高，没有额外空间进行分配担保，使用标记-清除或标记-整理算法。

#### 垃圾收集器  

  ... 后续学习...

### JVM的类加载机制

  + 什么是类加载机制
  
  JVM把描述类的数据从Class文件加载到内存中，并对数据进行校验、转换解析和初始化，最终形成可以被虚拟机直接使用的Java类型。
  
  + 什么时候执行类加载
  
  Java虚拟机不强制要求什么时候进行类加载的第一阶段：加载。但是对初始化阶段严格规定了5种情况。(加载与链接当然要在此之前开始)
    + 主动引用：
      + new 实例化对象、读取或设置类静态变量(final修饰的常量除外)、调用类静态方法。
      + 使用java.lang.reflect对类进行反射调用时。
      + 虚拟机启动，用户指定主类(包含main方法的那个类)，虚拟机会先初始化。
      + JDK1.7之后，如果一个java.lang.invoke.MethodHandle实例最后的解析结果REF_getStatic、REF_putStatic、REF_invokeStatic的方法句柄，并且这个方法句柄所对应的类没有进行过初始化，则需要先触发其初始化
    + 被动引用： 
      + 子类引用父类的静态字段，不会导致子类初始化。
      + 数组定义类引用，不会导致该类初始化。
      + final修饰的常量调用，不会触发初始化。
   
      ```java
      // 被动引用示例一：SuperClass init！ 123
      public class SuperClass {
        static { System.out.println("SuperClass init！"); }
        public static int value = 123;
      }
      class SubClass extends SuperClass {
        static { System.out.println("SubClass init！"); }
      }
      class NotInitialization {
        public static void main(String[] args) {
          System.out.println(SubClass.value);
        }
      }
      //被动示例二：
      SuperClass[] sca = new SuperClass[10];
      //被动示例三：
      public class ConstClass {
        static { System.out.println("ConstClass init"); }
        public static final String HELLO_WORLD = "hello";
      }
      class NotInitialization2{
        public static void main(String[] args) {
          System.out.println(ConstClass.HELLO_WORLD);
        }
      }
      ```
  
#### 类加载的过程
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-8-14/17327939.jpg)
  
  + 加载
    + 通过类的全限定名获取定义此类的二进制字节流。
    + 将字节流的静态存储结果转化为方法区的运行时数据结构。
    + 在内存中生成一个代表这个类的Class对象，作为方法区的这个类的各种数据的访问入口。

  + 验证
    确保Class文件的字节流符合JVM虚拟机的规范与安全要求。
    + 文件格式验证：验证Class文件的格式规范
      + 是否以魔数0XCAFEBABE开头
      + 主、次版本号是否在当前虚拟机处理范围之内。
      + 常量池中的常量是否有不被支持的常量类型
      + 等等...
    + 元数据验证：对字节码的描述信息进行语义分析，保证其描述符合Java语言规范
      + 验证这个类是否有父类(非Object)
      + 这个类的父类是否继承了不允许被继承的类(final修饰)
      + 如果该类非抽象类，是否实现父类或接口中的抽象方法。
      + 等等...
    + 字节码验证
    + 符号引用验证
    
  + 准备
    正式为类变量分配内存并设置类变量的初始值。这些类变量使用内存都在方法区分配。
    + 这里仅为类变量分配内存。实例变量是对象实例化时随对象分配在Java堆中。
    + 类变量的初始值是数据类型的零值，而非定义的值。(ConstantValue除外)
    
    ```java
    public static int a = 123;  // a 赋初值0，初始化阶段a才会被初始化123
    public final static int b = 1234; // 编译会为b生成ConstantValue属性，准备阶段直接为它赋值为1234
    ```
    
  + 解析    
    虚拟机将常量池内的符号引用替换为直接引用的过程。
    + 符号引用（Symbolic References）：符号引用以一组符号来描述所引用的目标，符号可以是任何形式的字面量，只要使用时能无歧义地定位到目标即可。
    + 直接引用（Direct References）：直接引用可以是直接指向目标的指针、相对偏移量或是一个能间接定位到目标的句柄。
  
  + 初始化
    初始化是类加载的最后一步。执行类构造器`<clinit>()`方法的过程。
    
    + 类加载的静态块只能访问定义在静态语句之前的变量，静态语句之后的变量只能赋值，不能访问。(非法向前引用)
    + 虚拟机会保证父类的`<clinit>`方法在子类的`<clinit>`方法前执行。
    + 如果类中没有静态块，也没有对变量的赋值操作，编译器不会生成`<clinit>`方法。
    + 接口也可能会有变量的赋值，但和类不同的是，接口的clinit方法不需要先执行父接口的。只有父接口的变量使用时，父接口才会初始化。

      
    
      

  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  

### JVM

  内存溢出有哪几种？
  
  垃圾收集有哪几种算法？
  
  JVM调优有哪些参数。
  
  如何查看程序中的死循环，命令是什么？
  
  JVM的原理。
  
  JVM类加载机制。
  
  堆和栈的区别
  
  jdk、jre，日志分析jconsole。
  
  jstack。
  
  内存模型，对象创建过程