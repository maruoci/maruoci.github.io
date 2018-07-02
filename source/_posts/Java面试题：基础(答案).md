---
title: Java面试题：基础(附答案)
date: 2018-06-28 11:45:22
categories:
- 面试题
tags:
- java
- 面试
- 基础
---

个人从网上整理筛选了近50道基础知识点题目，主要是基于基本语法、集合和面向对象相关后续再陆续添加。


### 语法基础

  基本数据类型、变量、运算符、字符串、控制流程
  
#### 知识点  

  == 和 equals的区别。
  
  1、== 属于比较运算符，equals是方法
  2、== 和 Object的equals方法等同，String重写了equals方法，比较的是内容。
  
  final, finally, finalize 的区别
  
  final 修饰符，可修饰类、变量、方法和方法参数
    被final修饰的类不可被继承，比如String
    被final修饰的变量，当变量属基本类型时代表该变量为一个常量；当变量为引用类型时，不可改变的只是变量引用。
    被final修饰的方法，可被继承但不可被重写override。
  finally是修饰代码块，与try/catch一并使用,不考虑try catch 代码块是否会抛异常，finally代码最终执行。
  finalize是Object的方法， 垃圾回收再GC清理内存前执行的方法。提供给程序做些额外的清理工作。

  String s=new String(“xyz”);创建了几个字符串对象?
  
  两个，"xyz" 常量在常量池，new String("xyz")的对象放在 heap中，最后将引用s指向 heap中的对象。
  
  静态变量和实例变量的区别。
  
  静态变量被static修饰，属于类，所以也叫类变量。而实例变量属于类的实例对象。
  JVM中，静态变量同class一起加载，在实例加载之前，且内存中仅一个。实例变量则随类实例创建多少就有多少对象。
    
  值传递、引用传递的区别。
  
  这里应该问的是call by value值调用和引用调用call by reference。
  值调用：方法接收调用者传过来的值； 引用调用：方法接收调用者提供的变量地址。
  Java是采用的值调用。
  
  ```java
    class CallTest{

    public String name ;

     public static void main(String[] args){
       int a = 10;
       baseTypeTest(a);
       
       CallTest t = new CallTest();
       t.name = "java";
       changeName(t);
       
       changeObj(t);
     }
     
     //基本类型
     public static void baseTypeTest(int a){
       a = 20;
     }
     
     //引用类型
     public static void changeName(CallTest t){
       t.name = "python";
     }
     
     //引用类型2
     public static void changeObj(CallTest m){
       m = new CallTest();
       m.name = "NONE";
     }
    }
  ```
  
  代码说明：
    1、baseTypeTest 接收基本类型int的参数a, 将a赋值20，主函数的a并没有发生变化，所以是值调用。
    2、changeName 接收引用类型CallTest参数t，修改t的name值为python，主函数的name也发生变化，这里是引用调用吗？
    3、肯定不是，如果是引用调用的话，changeObj 函数就会将t 修改为m，name值变为NONE了。
  
  Java的四种引用方式(强引用、软引用、弱引用、虚引用)
  
  强引用是我们常用的变量引用方式，a = new Object();强引用关联的对象JVM即使是OOM也不会回收该对象。
  软引用是指一些可有可无的对象，用lang.ref包里的类表示，当内存不足时会回收这类对象。
  弱引用是指一些可有可无的对象，用lang.ref包里的类表示，JVM垃圾回收时回收这类对象(不考虑内存是否不足)。
  虚引用它并不影响对象的生命周期，一个对象与虚引用关联，则跟没有引用与之关联一样。
  
  char 型变量中能不能存贮一个中文汉字?
  
  可以，char占两个字节，Unicode编码的部分汉字是两个字节的。
  
  byte的数值范围是多少
  
  byte长度1字节，为8位。首位是符号位，0 代表正，1代表负。
  所以最大值为0111 1111 也就是2^7-1=127 负位最大1000 0000是-128（此处负数要弄清楚原码，反码，补码）
  
  最有效的计算2*8的方法
  
  2 << 3
  
  switch是否能作用在byte上，是否能作用在long上，是否能作用在String上?
  
  switch可以作用在char,byte,short,int和他们的包装类；
  switch可以作用在枚举；
  switch可以作用在String上(java7以后)；
  switch不可以作用在double、float、long、boolean上。
  
  String为什么要设计成不可变的
  
  1、final修饰避免被继承。
  2、String内部实际是一个私有的final字符数组，这样加上类的不可继承。就避免了数据被破坏。
  3、不可变的String数据更安全（HashSet、线程安全）。 
   
#### 代码题  
  
  下面两行代码变量s的运行结果是否有区别,为什么。
  
```
short s=1; s=s+1;
short s=1; s+=1
```
  
  s = s + 1 ; s + 1 会隐式转换为int类型， 把int类型赋值给short，会丢失精度，需要强转。
  s = s + 1 ; s +=1 += 属于操作符，系统在执行时会自动强转。
  
  下面两行代码`a==b`的运行结果是否有区别,为什么。
  
```
  Integer a,b = 100;  a==b
  Integer a,b = 200;  a==b
``` 
    
  int的包装类在-128-127之间的值是一个IntegerCache数组，这个区间内的数值会复用，== 值为true。
  -128--127之外的数据都是不同的对象引用，== 值为false
  
  float f = 3.4; 是否正确？
  
  3.4默认属于double类型，double转float会丢失精度，需要强转。
  
### 集合类

  List 、 Set 和 Map 三者区别
  
  List、Set都继承了Collection接口，Map没有
  List、Set是元素存储，Map是键值对存储的。
  
  ArrayList 、LinkedList 与 Vector 的区别
  
  ArrayList与Vector都是对象数组存储，LinkedList是双向链表的存储方式。
  因为是数组存储所以ArrayList与Vector在随机查询时效率较高，LinkedList因为查询时移动指针效率较低
  Linked基于双向链表存储，LinkedList的插入或删除节点效率较高，ArrayList因为有一个数组拷贝的过程，效率较低。
  ArrayList是线程不安全的，Vector因为使用synchronized修饰，所以是线程安全的。
  Vector可以设置扩容大小,默认为原来的2倍，ArrayList不支持设置扩容大小，扩容1.5倍，Vector双向链表不存在扩容问题。
  
  HashMap 和 HashTable 的区别
  
  HashMap 继承AtstractMap ， HashTable继承Dictionary
  HashMap 不是线程安全的，HashTable被synchronized修饰，所以是线程安全的。
  HashMap 可以存储null的key和value，HashTable不可以。
  HashTable 存在一个contains方法和containsValue方法，HashMap因为避免歧义去掉了contains方法。
  HashMap存储时重写了hash方法，HashTable使用的Object的hash方法。
  
  HashSet 和 HashMap 区别
  
  HashSet是实现Set接口，HashMap是实现Map
  HashSet存储对象，HashMap存储键值对
  HashSet添加对象add方法，HashMap是put方法
  
  HashMap 的工作原理及代码实现
  
  基于数组+链表的形式存储数据，默认容量(桶)大小是16，map里有一个默认增长因子为0.75
  当桶的数量大于16*0.75=12时，会触发扩容。或者当桶中数据超过默认的8个且此时桶的个数小于默认64个时也会触发扩容。
  当桶中数据超过8个，且桶个数超过64时，桶的数据存储由链表转为红黑树。
  
  HashMap在添加元素时，会使用key的hash值 和 桶个数 n 去做位运算。 hash(key) & (n-1) 碰撞获取桶的位置，并存储。
  hash的计算采用高16位^低16位的方式来计算(h=key.hash()) ^ ( h>>>16 )，使数据的分布更均匀。
  当key为null时，默认存在第一个桶。
  
  HashMap 和 ConcurrentHashMap 的区别
  
  ConcurrentHashMap 的工作原理及代码实现
  
  Collections.synchronizedMap 与 ConcurrentHashMap的区别

### 面向对象(封装、继承、多态)

  面向对象的三大特征与理解。
  
  封装：隐藏实现细节，使代码模块化。
  继承：is-a的从属关系，子类继承父类的非私有变量及方法，提高代码复用。
  多态：主要是重写与重载。重载属于一种编译期的多态体现，重写属于运行时多态。不同类对同一消息做的不同响应。
      实现方式是子类继承或实现父类，重写父类方法或接口。父类引用指向子类对象。这样相同类型的父类引用不同的子类对象，
      对同一方法做出不同的响应。但这种对于子类独有方法就不管用了。另外因为java单继承的原因。接口的多态体现比类的多态
      有更高的灵活性。
  
  自动装箱与自动拆箱。
  
  自动装箱，就是系统自动将基本数据类型转换为包装类的过程，比如`Integer i =10`等同于`Integer i = Integer.valueOf(10)`
  自动拆箱，反之。
  
  向上转型与向下转型的理解。
    
  父类引用指向子类对象为向上转型。反之为向下转型。
  下面记录了一个比较经典的代码题，可加深理解。
  
  ```java
  public class OverrideTest2 {
  
    public static void main(String[] args) {
      A a1 = new A(); A a2 = new B();
      B b = new B();  C c = new C() ; D d = new D();
      System.out.println("1--" + a1.show(b));System.out.println("2--" + a1.show(c));
      System.out.println("3--" + a1.show(d));System.out.println("4--" + a2.show(b));
      System.out.println("5--" + a2.show(c));System.out.println("6--" + a2.show(d));
      System.out.println("7--" + b.show(b));System.out.println("8--" + b.show(c));
      System.out.println("9--" + b.show(d));
    }
  }
  
  class A {
    public String show(D obj) { return ("A and D"); }
    public String show(A obj) { return ("A and A"); }
  }
  class B extends A{
    @Override
    public String show(D obj) { return ("A and D"); }
    public String show(B obj) { return ("B and B"); }
    @Override
    public String show(A obj) { return ("B and A"); }
  }
  class C extends B{ }
  class D extends B{ }

  ```
  
  java中实现多态的机制是什么。
  
  java实现多态主要分两种，编译时多态与运行时多态。
  编译时多态 主要体现在 方法重载overload。同样的方法具有不同的方法签名，形如`add(int index, E element)` 无返回类型。
  运行时多态 主要体现在 方法重写override。主要有两种方式，extend继承与implements实现接口。
  
  访问修饰符的区别。
  
  public    全包
  protected 本包 + 子类
  default   本包
  private   本类
  
  抽象类与接口的区别。
  
  抽象类可以有具体的方法实现，接口的方法全是抽象方法。
  抽象类的变量没有限制，接口的变量都是public static final修饰的。
  抽象类可以有静态变量与静态代码块，接口不可以。
  一个类只可以继承一个抽象类，但可以实现多个接口。
  
  抽象类特点及应用场景。
  
  抽象类是对事物的抽象；
  接口是对行为的抽象。
  
  构造器Constructor是否可被override。
  
  接口是否可继承接口? 抽象类是否可实现接口? 抽象类是否可继承实体类。
  
  接口可继承接口，抽象类可实现接口，抽象类可继承实体类。
  
  abstract的method是否可同时是static,是否可同时是native，是否可同时是synchronized。
  
  都不可以。
  
  自定义类，构造器函数是否必须的。
  
  不必须。
  
  是否可以从一个static方法内部发出对非static方法的调用。
  
  不可以。

### 异常

   常见的运行时异常有哪些？
  
   NullPointerException、IndexOutOfBoundsException、NumberFormatException、ClassCastException、
   ArithmeticException、IllegalArgumentException
  
   异常分类及处理机制。
   
   异常的父类为Throwable，下一层的子类有Error、Exception、StackRecorder(其他包的)。
   Error是靠程序无法恢复的错误，比如内存不足等。遇到此类错误大多数建议程序中止。
   Exception表示程序可以处理的异常，遇到此类异常应该尽可能的去处理。
       
   处理机制 throw、throws、try...catch、系统自动抛出。
      
   Exception RuntimeException 区别
   
   另外Exception类还可以分为：受检异常和非受检异常。
    RunTimeException运行时异常属于非受检异常，程序可能出现此异常时，JVM不会检查他。
      此类异常一般是程序编程不当引起的，应当修改代码尽量避免。
    受检异常如IOException、InterruptedException需要try...catch捕获或者throws向上抛出，否则编译报错。
    此类异常为程序可以处理的。如果方法本身不能处理， 调用者需要处理使程序正常运行，不至于让程序中止。
   
   Error与Exception区别  
   
   Error和RuntimeException,JVM都不会去检查此类异常。出现此类异常，程序都会中止。
   他们不同在于Error一般是JVM抛出的错误，RuntimeException则是程序代码错误。

### 泛型、反射、注解、动态代理  
  
   泛型的特点及使用场景。
   
   泛型的特点：
      以List为例，泛型可以将类型转换异常从运行期提前到编译期。	
      泛型只在编译时期有效，编译后的字节码文件中不存在有泛型信息(泛型擦除)。 
   泛型的使用场景：抽象类、接口
   
   List <T> 与 List <?> 的区别
   
   List<T> 用于声明一个泛型类的类型参数。List<?>不能声明类型参数。
   List<?>主要在于使用泛型方法时不确定返回类型，?表示无限制通配符，参数可以是任意类型。
   
   除了使用new创建对象之外，还可以用什么方法创建对象。
   
   Class类的newInstance()方法
   clone()方法，需实现Cloneable接口并重写clone()方法
   反序列化。
   
   反射的特点及使用场景。
   
   运行时获取类的实例、方法、属性与注解并且能够调用类的方法。这个在框架比如xml配置文件上面体现较多，降低了类之间的耦合，更灵活。
   反射的使用场景：加载数据库驱动、xml配置文件等。
   
   Java反射创建对象效率高还是通过new创建对象的效率高。
   
   ...
   
   java反射的原理。
   
   ...
   
   JAVA序列化。
   
   Serializable 和Parcelable 的区别。
   
   说说自定义注解的场景及实现

### 内部类、匿名内部类、IO、其他
    
   内部类与匿名内部类的区别
   
   Java8的新特性   

