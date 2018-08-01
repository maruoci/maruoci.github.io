---
title: Java面试题：基础
date: 2018-06-28 09:32:22
categories:
- 面试题
tags:
- Java
- 面试
---

个人整理筛选了近50道基础知识点题目，主要是基于基本语法、集合和面向对象相关后续再陆续添加。


### 语法基础

  基本数据类型、变量、运算符、字符串、控制流程
  
#### 知识点  

  = 和 equals的区别。
  
  final, finally, finalize 的区别

  String s=new String(“xyz”);创建了几个字符串对象?
  
  静态变量和实例变量的区别。
  
  值传递、引用传递的区别。

  <!-- more -->
  
  Java的四种引用方式(强引用、软引用、弱引用、虚引用)
  
  char 型变量中能不能存贮一个中文汉字?
  
  byte的数值范围是多少
  
  最有效的计算2*8的方法
  
  switch是否能作用在byte上，是否能作用在long上，是否能作用在String上?
  
  String为什么要设计成不可变的
   
#### 代码题  
  
  下面两行代码变量s的运行结果是否有区别,为什么。
  
  ```
  short s=1; s=s+1;
  short s=1; s+=1
  ```
  
  下面两行代码`a==b`的运行结果是否有区别,为什么。
  
  ```
    int a,b = 100;  a==b
    int a,b = 200;  a==b
  ```  
  
  float f = 3.4; 是否正确？
  
  
### 集合类

  List 、 Set 和 Map 三者区别
  
  ArrayList 、LinkedList 与 Vector 的区别
  
  HashMap 和 HashTable 的区别
  
  HashSet 和 HashMap 区别
  
  HashMap 的工作原理及代码实现
  
  HashMap 和 ConcurrentHashMap 的区别
  
  ConcurrentHashMap 的工作原理及代码实现
  
  Collections.synchronizedMap 与 ConcurrentHashMap的区别

### 面向对象(封装、继承、多态)

  面向对象的三大特征与理解。
  
  自动装箱与自动拆箱。
  
  (多态)向上转型与向下转型的理解。
  
  java中实现多态的机制是什么。
  
  访问修饰符的区别。
  
  抽象类与接口的区别。
  
  抽象类特点及应用场景。
  
  构造器Constructor是否可被override。
  
  接口是否可继承接口? 抽象类是否可实现接口? 抽象类是否可继承实体类。
  
  abstract的method是否可同时是static,是否可同时是native，是否可同时是synchronized。
  
  自定义类，构造器函数是否必须的。
  
  是否可以从一个static方法内部发出对非static方法的调用。
  


### 异常

   常见的运行时异常有哪些？
  
   异常分类及处理机制。
  
   Exception RuntimeException 区别
   
   Error与Exception区别  

### 泛型、反射、注解、动态代理  
  
   泛型的特点及使用场景。
   
   List <T> 与 List <?> 的区别
   
   除了使用new创建对象之外，还可以用什么方法创建对象。
   
   反射的特点及使用场景。
   
   Java反射创建对象效率高还是通过new创建对象的效率高。
   
   java反射的原理。
   
   JAVA序列化。
   
   Serializable 和Parcelable 的区别。
   
   说说自定义注解的场景及实现

### 内部类、匿名内部类、IO、其他
    
   内部类与匿名内部类的区别
   
   Java8的新特性   

