---
title: Jenkins使用Git构建Maven任务
date: 2018-09-27 10:49:25
categories:
- Jenkins
tags: 
- Jenkins
- Git
- Maven
---

> 上一节，我们已经把环境准备好了并且也成功的启动了Jenkins。下面我们开始尝试使用git配置一个maven项目的任务。

<!-- more -->

## Jenkins系统配置

  主页 --> 系统管理 --> 全局工具配置

 **配置Java**
 
 ![](http://pbsg2r9io.bkt.clouddn.com/18-9-27/80455003.jpg)
 
 安装路径为我们Jenkins服务器上面的Java路径
 
 **配置Git**
 
 ![](http://pbsg2r9io.bkt.clouddn.com/18-9-27/29999650.jpg)
 
 **配置Maven**
 
 ![](http://pbsg2r9io.bkt.clouddn.com/18-9-27/50175150.jpg)
 
 ![](http://pbsg2r9io.bkt.clouddn.com/18-9-27/84464364.jpg)
 
 Maven这里有两处配置，一个是Maven的安装路径，一个是配置文件的引入。

## 开始第一个任务吧

  主页 --> 新建任务，选择'构建一个maven项目'，点击确定
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-9-27/59070219.jpg) 
  
  > 新建的时候如果没能找到这个新建maven项目的选项，需要安装Maven Integration 插件
  
  主页 --> 系统管理 --> 插件管理
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-9-27/9295638.jpg)
  
  回到上面，确定后进入下一页，点击‘源码管理’，选择Git
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-9-27/92246908.jpg)
  
  > 在验证的时候，可能会报`Host key verification failed`git验证异常，这里可选择用户密码验证或上传私钥解决。
  
  选择Build
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-9-27/71315727.jpg)
  
  到这里一个简单的任务就创建完毕了。点击保存回到任务面板页，点击立即构建。
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-9-27/28187921.jpg)
  
  点开控制台输出，可以看到详细的构建日志。
  
  如果一切顺利看到 Build Success的日志后，我们就可以回到工作空间看到我们的项目包了。
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-9-27/40841346.jpg)

## 小结
  
  这里只是一个简单的构建，至于Jenkins的一些复杂配置包括打包后的服务自动部署等等并未涉及。  
  
  