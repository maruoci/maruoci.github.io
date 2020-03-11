---
title: RabbitMQ：安装
date: 2020-03-11 16:47:42
categories:
- RabbitMQ
tags:
- RabbitMQ
---

> rabbitMQ是一个在AMQP协议标准基础上完整的，可服用的企业消息系统。它遵循Mozilla Public 
> License开源协议，采用 Erlang 实现的工业级的消息队列(MQ)服务器，Rabbit MQ 是建立在Erlang OTP平台上。

## Windows下的安装

### Erlang的安装

 RabbitMQ是Erlang语言编写，所以安装RabbitMQ要安装Erlang。

 下载地址： [官网](http://www.erlang.org/downloads)

 ![](https://raw.githubusercontent.com/maruoci/images/master/Java/RabbitMQ/mq-setup-01.png)

 官网地址下载较慢，建议从RabbitMQ官网下载。[https://www.erlang-solutions.com/resources/download.html](https://www.erlang-solutions.com/resources/download.html),下载完成，一路默认安装即可。
 
 设置环境变量,增加系统变量：ERLANG_HOME
 
 ![](https://raw.githubusercontent.com/maruoci/images/master/Java/RabbitMQ/mq-setup-01.jpg)
 
 修改环境变量Path，追加：%ERLANG_HOME%\bin;

### RabbitMQ安装

 RabbitMQ[下载](http://www.rabbitmq.com/download.html)有[安装包](http://www.rabbitmq.com/install-windows.html)和[解压包](http://www.rabbitmq.com/install-windows-manual.html)，此处选择解压包下载。

 ![](https://raw.githubusercontent.com/maruoci/images/master/Java/RabbitMQ/mq-setup-03.jpg)

 设置环境变量，增加系统变量RABBITMQ_SERVER
 
 ![](https://raw.githubusercontent.com/maruoci/images/master/Java/RabbitMQ/mq-setup-04.jpg)
 
 修改环境变量Path，追加：%RABBITMQ_SERVER%\sbin;
 
### 启动RabbitMQ

 cmd切换到rabbitMQ的安装目录下的sbin目录，输入rabbitmqctl status
 
 ![](https://raw.githubusercontent.com/maruoci/images/master/Java/RabbitMQ/mq-setup-05.jpg)
 
 此时说明rabbitMq未启动，安装插件，在sbin目录输入rabbitmq-plugins.bat enable rabbitmq_management。
 
 ![](https://raw.githubusercontent.com/maruoci/images/master/Java/RabbitMQ/mq-setup-06.jpg)
  
 启动rabbitMq，输入命令：rabbitmq-server.bat

 ![](https://raw.githubusercontent.com/maruoci/images/master/Java/RabbitMQ/mq-setup-07.jpg)
 
 rabbitmq启动成功，浏览器中访问：http://localhost:15672
 
 ![](https://raw.githubusercontent.com/maruoci/images/master/Java/RabbitMQ/mq-setup-08.jpg)
 
 输入guest,guest进入rabbitMQ管理控制台：
 
 ![](https://raw.githubusercontent.com/maruoci/images/master/Java/RabbitMQ/mq-setup-09.jpg)
 
 至此，rabbitMQ安装部署完成。
 
 
 