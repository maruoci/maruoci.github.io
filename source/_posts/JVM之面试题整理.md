---
title: JVM学习(十)：面试题整理
date: 2018-10-27 22:55:30
categories:
- Java虚拟机
tags:
- Java
- Jvm
---

> 从网上和书上整理出来的一些面试笔试题目，后面也会针对题目整理出网上答案和个人的理解。

## 单选题

1. 下面代码运行结果是：

   ```java
    String str1 = new StringBuilder("计算机").append("软件").toString();
    System.out.println(str1.intern() == str1);
    String str2 = new StringBuilder("jav").append("a").toString();
    System.out.println(str2.intern() == str2);
   ```
   
   A: true false
   B: false true
   C: true true
   D: false false

<!-- more -->

2. 下面代码运行结果是： 

   ```java
    String s = new String("1");
    s.intern();
    String s2 = "1";
    System.out.println(s == s2);

    String s3 = new String("1") + new String("1");
    s3.intern();
    String s4 = "11";
    System.out.println(s3 == s4);
   ```
   
   A: true false
   B: false true
   C: true true
   D: false false       
   
3. 下面代码运行结果是： 

   ```java
    String s = new String("1");
    String s2 = "1";
    s.intern();
    System.out.println(s == s2);

    String s3 = new String("1") + new String("1");
    String s4 = "11";
    s3.intern();
    System.out.println(s3 == s4);
   ```
   
   A: true false
   B: false true
   C: true true
   D: false false       

## 单选题（答案）
    
1. D 或 A

  ```java
  // 在常量池创建一个"计算机"、一个"软件"对象，并在堆内创建一个"计算机软件"对象
  String str1 = new StringBuilder("计算机").append("软件").toString();
  // JDK1.6 str1.intern 常量池拷贝一个堆内的"计算机软件"对象 故 此处false
  // JDK1.7 str1.intern 常量池创建一个对象引用，引用堆内创建的对象，故 此处为true
  System.out.println(str1.intern() == str1);
  String str2 = new StringBuilder("jav").append("a").toString();
  // java 常量 在JVM启动的时候就会将其放进常量池，故在JDK1.6 和 JDK1.7中 都是false
  System.out.println(str2.intern() == str2);
  ```    
2. D 或 B
3. D  
   