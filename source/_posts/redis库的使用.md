---
title: redis库的使用
date: 2018-11-22 23:28:30
categories:
- Python
tags:
- Python
---

> Python的redis库的安装与基本使用

## 参考资料

 **Python3网络爬虫开发实战-崔庆才** 
 
<!-- more --> 

## redis基于Windows的安装

 Github下可以下载发行版本([地址](https://github.com/MSOpenTech/redis/releases))。
 
 下载Redis-x64-3.2.100.msi 安装即可。

## reids-py的安装

 redis-py的GitHub[地址](https://github.com/andymccurdy/redis-py)。
 
 `pip install redis -i https://pypi.douban.com/simple` 安装库
 
 验证安装
 
 ```
  >>> import redis
  >>> redis.VERSION
  (3, 0, 1)
  >>>
 ```
  
> redis-Dump是一个用于redis数据导入/导出的工具。基于Ruby实现，如果有导入/导出需求 可以尝试安装，不是必需的。
> 安装redis-dump需要先安装Ruby。这里不赘述，具体可查看[教程](http://www.runoob.com/ruby/ruby-installation-windows.html)

## 简单示例

  redis-py库两个比较核心的操作类为Redis和StrictRedis。
  
  ```python
    from redis import StrictRedis
    import redis as db
    
    redis = StrictRedis(host='localhost', port=6379, db=2)
    redis.set('python-name', 'python-value-2018/11/24')
    print(redis.get('python-name'))
    
    redis2 = db.Redis(host='localhost', port=6379, db=2)
    print(redis2.get('python-name'))
  ```
 
## 相关链接

  **[官网](https://redis.io)** 
  **[官方文档](https://redis.io/ddocumentation)** 
  **[中文官网](http://www.redis.cn)** 
  **[GitHub](https://github.com/antirez/redis)** 
  **[中文教程](http://runoob.com/redis/redis-turorial.html)** 
  **[Redis Desktop Manager](https://redisdesktop.com)** 
  **[Redis Desktop Manager GibHub](https://github.com/uglide/RedisDesktopManager)** 