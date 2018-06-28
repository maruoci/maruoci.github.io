---
title: Java面试题：并发
date: 2018-06-28 10:40:10
categories:
- 面试题
tags:
- java
- 面试
- 并发
- 多线程
---

  个人从网上整理筛选了近30道多线程相关面试题目。
  
### 多线程

  为什么要使用多线程。
  
  创建线程的方式。
  
  进程与线程的区别。
  
  线程的生命周期。
  
  可以直接调用Thread类的run()方法么
  
  如何让正在运行的线程暂停一段时间
  
  如何停止一个正在运行的线程
  
  对线程优先级的理解。
  
  T1、T2、T3三个线程，你怎样保证T2在T1执行完后执行，T3在T2执行完后执行。
  
  线程之间是如何通信的。
  
  为什么Thread类的sleep()和yield()方法是静态的。
  
  如何确保线程安全。
  
  volatile关键字在Java中有什么作用。
  
  什么是死锁、活锁、饥饿锁。
  
  ThreadLocal解决了什么问题。

### 并发

  什么是原子操作？在Java Concurrency API中有哪些原子类(atomic classes)？

  Java Concurrency API中的Lock接口(Lock interface)是什么？对比同步它有什么优势？
  
  什么是Executors框架？

  什么是阻塞队列？如何使用阻塞队列来实现生产者-消费者模型？
  
  什么是Callable和Future?
  
  什么是FutureTask?
  
  什么是并发容器的实现？
  
  Executors类是什么？
  
  为什么说ConcurrentHashMap是弱一致性的？以及为何多个线程并发修改ConcurrentHashMap时不会报ConcurrentModificationException？
  
  什么是可重入锁（ReentrantLock）？
  
  当一个线程进入某个对象的一个synchronized的实例方法后，其它线程是否可进入此对象的其它方法？

  普通方法可以，同步方法需要等待
  
  乐观锁和悲观锁的理解及如何实现，有哪些实现方式？
  
  线程池的子线程抛异常后，线程池中的线程会减少吗
  
  ThreadPoolExecutor构造函数的参数解释