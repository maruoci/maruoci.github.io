---
title: Java面试题：Web(答案)
date: 2018-07-10 19:40:10
categories:
- 面试题
tags:
- Java
- 面试
- Spring
---

  个人从网上整理筛选了近30道web及Spring相关面试题目。
  
### Web

  什么是servlet
  
  servlet的生命周期
  
  init(),service(),destroy()

  Http的get与post请求的区别。

  http请求到服务器经历过那些流程

  <!-- more -->
  
  1.浏览器dns解析请求ip
  2.浏览器向ip所在服务器建立TCP连接
  3.连接后，浏览器发送请求命令 
  4.浏览器发送http请求头和正文等信息。
  5.服务器应答。应答协议及应答状态码。
  6.服务器发送应答头信息和响应数据，以空白行表示应答头信息结束，以content-type的描述格式返回响应数据。
  7.服务器关闭tcp连接。
  
  我们在web应用开发过程中经常遇到输出某种编码的字符，如iso-8859-1等，如何输出一个某种编码的字符串？
  
  new String(str.getBytes("ISO-8859-1"), "UTF-8");
  
  几种会话跟踪技术？
  
  cookie和session的作用、区别、应用范围

  tcp、udp的特点
  
### Spring

  对spring框架的理解
  
  Spring的核心模块讲解
  
  Core Container： Core、Beans、Context、SpEL
  AOP
  Aspects
  Web：Servlet、Web、WebSocket、Portlet
  DataAccess: JDBC、ORM、OXM、JMS、Transactions
  Instrumentation
  Messaging
  Test
    
  Spring IOC的作用与原理。
  
  IOC Inversion Of Control 控制反转。
  资源不由使用资源的双方管理，而由不使用资源的第三方管理，这可以带来很多好处。
  第一，资源交给容器集中管理，实现资源的可配置和易管理。
  第二，降低了使用资源双方的依赖程度，也就是降低了耦合度。
  
  spring容器启动时读取所有需要spring管理的bean配置，容器如果使用ApplicationContext，
    容器在启动的时候会把所有单例的实例提前实例化到容器中。实例化是基于反射生成的，
    加载实例的时候会根据我们在xml文件里或是注解声明的对象依赖关系去把对象实例给注入进去。
  Spring Aop的作用及原理，好处。
  
  IOC 与 DI 的区别
  
  IOC控制反转是一种概念，对象不关心依赖对象的创建，交给容器管理，DI依赖注入是实现的细节。通过依赖注入实现控制反转。
    
  SpringMVC的执行流程。
  
  FactoryBean 与 BeanFactory 的区别。
  
  BeanFactory是spring的核心，管理所有需要容器管理的bean，负责bean的管理与创建。
  FactoryBean是一种工厂bean，也是由BeanFactory来管理，容器对它的实例返回的不是对象本身而是对象工厂生成的对象。
  
  BeanFactory和ApplicationContext的区别？
  ApplicatonContext上下文是BeanFactory的子类，相对BeanFactory它增加了对SPEL、属性编辑器、国际化等的支持。
  另外，BeanFactory容器加载时不负责bean的注册，只有在使用时才会实例化，而ApplicationContext在容器加载初期就会对大部分的bean进行实例化。
    
  Spring Bean 的生命周期
  
  1、容器加载初期，创建一个DefaultListableBeanFactory，这个就是我们的bean工厂了。
  2、容器创建好以后，创建一个XmlBeanDefinitionReader去读取Bean的配置。将bean的信息由xml转换为BeanDefinition。
  3、实例化BeanFactoryPostProcessor 这个可以修改bean的元数据。
  4、实例化BeanPostProcessor相关类。
  5、实例化bean并注入属性。实例化前后提供了BeanPostProcessor可以做一些扩展。
  6、容器使用完毕，后就销毁了。
  
  解释Spring支持的几种bean的作用域
  singleton
  prototype
  web环境下支持request、session作用域的实例
  
  Spring框架中的单例bean是否是线程安全的
  
  spring单例bean一般都是无状态bean，所以可以理解为线程安全的。
  
  请举例说明如何用Spring注入一个Java的集合类
  
  请举例说明@Autowired注解
  
  请举例说明@Required注解
  
  Spring支持的事务管理类型
  
  编程式事务，显式开启提交或回滚事务，极大的灵活性，难以维护。
  声明式事务，基于aop的基础，可以通过注解或xml来管理在目标方法执行前创建或加入事务，执行完成根据执行情况提交或回滚。
  
  Spring框架的事务管理有哪些优点
  
  对不同的api进行统一编程模型，如JTA,JDBC,Hibernate,JPA,JDO...
  支持声明式事务
  简化编程式事务api
  对spring数据层的完美抽象
  
  Spring 是如何管理事务的，事务管理机制？
  
  spring 事务的特性 ACID
  A:Atomicity 原子性，事务是一个原子操作，要么全部成功要么全部失败。
  C:Consistency 一致性，
  I:Isolation 事务与事务之间应隔离开，防止数据顺坏。
  D:Durability 持久性，事务的结果应持久化存储，一旦事务完成，无论什么错误都不影响事务的结果。
  
  
  Spring 的不同事务传播行为有哪些，干什么用的？ PROPAGATION
  
  PROPAGATION_REQUIRED:     如果存在一个事务，则支持当前事务，如果不存在则创建一个。
  PROPAGATION_SUPPORTS:     如果存在一个事务，则支持当前事务，如果没有则非事务执行。
  PROPAGATION_MANDATORY:    如果存在一个事务，则支持当前事务，如果没有抛异常。
  PROPAGATION_REQUIRED_NEW：总会开启一个新的事务。
  PROPAGATION_NOT_SUPPORTED：如果存在一个事务，则挂起，总是非事务执行。
  PROPAGATION_NEVER:        如果存在一个事务，则抛异常，总是非事务执行。
  PROPAGATION_NESTED:       如果存在一个事务，则运行在一个嵌套事务里，如果没有事务，则按PROPAGATION_REQUIRED处理。
  
  Spring 事务的隔离级别。  
  
  并发事务可能导致的问题：
  脏读：事务A读取到事务B未提交的数据，如果B回滚事务，A读取的数据为无效数据。
  不可重复读：一个事务多次读取的数据不一致，因为事务B在事务A读取两次的间隔，更新了数据。
  幻读：一个事务多次读取的数据不一致，事务B在事务读取多次的间隔，新增或删除了数据。
  
  Spring的隔离级别：
  ISOLATION_DEFAULT:      使用后端数据库默认隔离级别。
  ISOLATION_UNCOMMITED:   最低隔离级别，允许读取未提交数据变更，可能导致脏读，不可重复读和幻读。
  ISOLATION_COMMITED:     允许读取已提交的数据，可以阻止脏读，可能导致不可重复读，幻读。
  ISOLATION_REPEATABLE_READ:对同一字段多次读取结果一直，除非数据被本事务修改，可阻止脏读与不可重复读，可能导致幻读。
  ISOLATION_SERIALIZABLE: 最高的隔离级别。完全服从ACID的隔离级别，阻止脏读，不可重复读和幻读。
  
###　Spring boot

  Spring boot 、Spring Mvc 、 Spring 三者之间的关系  