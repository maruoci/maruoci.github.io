---
title: Java面试题：DB
date: 2018-06-28 11:20:10
categories:
- 面试题
tags:
- Java
- redis
- Mysql
- 面试
---

  个人从网上整理筛选了近30道相关面试题目。
  
### MySql

  mysql与oracle的区别。

  数据库sql语句的优化。

  sql语句中的执行顺序。

  Mysql数据库3种存储引擎有什么区别？

  left join、right join、inner join 区别。
  
  mysql与oracle分页查询。
  
  数据库优化、当一个表中的数据超过1亿条时有什么优化方案

### 场景SQL
 
 统计每个班级有多少学生。学生表：student
 
  student_id,		class_id,		name
    1			      classA			Lisi
    2			      classA      Mark
    3			      classB			jack
    4			      classC			lucy
    5			      classC			Frank		
 
 设计一套CMS系统或博客的表结构（UML图即可）

### Redis  
  
  redis特点
  
  redis的优点
  
  redis有什么数据类型?都在哪些场景下使用
  
  redis和 memcheched什么区别?
  
  redis 相比memcached的优势。
  
  mySQL里有2000w数据，redis中只存20w的数据，如何保证redis中的数据都是热点数据
  
  reids的主从复制是怎么实现的? redis的集群模式是如何实现的呢? redis的key是如何寻址的
  
  使用redis如何设计分布式锁?使用zk可以吗?如何实现啊?这两种哪个效率更高
  
  知道 redis的持久化吗?都有什么缺点优点啊?具体底层实现呢
  
  redis过期策略都有哪些?写一下java版本的代码