---
title: Redis学习
date: 2018-12-25 18:10:30
categories:
- Redis
tags:
- Redis
---

## Redis初步了解

### Redis的特性

**速度快**

  1. Redis的数据存放在内存中，这个是Redis速度快的最主要原因。
  2. Redis是C语言编写，C的程序距操作系统最近，速度相对更快。
  3. Redis使用单线程架构，预防了多线程可能产生的竞争问题。
  4. Redis是集性能与优雅于一身的开源代码。
  
  以上4点为Redis速度快的主要原因，官方数据Redis读写性能可以达到10W/秒。

**基于键值对的数据库**

  Redis基于键值对的数据库，与其他键值对数据库不同的是，Redis的值不仅仅是字符串，还可以是具体的数据结构。

> Redis全称REmote Dictionary Server。主要提供字符串、哈希、列表、集合、有序集合5种数据结构。
> 在字符串基础上还演变出了位图(Bitmaps)和HyperLogLog两种数据结构。

**丰富的功能**

  1. 提供键过期功能，可以实现缓存。
  2. 提供发布订阅功能，可以实现消息系统。
  3. 支持Lua脚本，可以利用Lua创造新的Redis命令。
  4. 提供简单事务功能，在一定程度保证事务特性。
  5. 提供流水线(Pipeline)功能，客户端可以一批命令一次性传到Redis，减少网络开销。

**简单稳定**
  
  1. Redis源码3.0版本后添加集群特性，大概5W行代码，代码量少很多。
  2. Redis单线程模型不光服务端模型简单，客户端开发也简单。
  3. Redis不需要依赖操作系统类库，Redis自己实现了事件处理相关功能。

**客户端语言多**

  Redis提供了简单的TCP通信协议，很多编程语言可以方便接入Redis。包含：Java、Python、C、C++、NodeJS等。

**持久化**
  
  Redis提供RDB和AOF两种方式来对内存数据进行持久化。

**主从复制**

  复制功能是Redis分布式的基础，Redis提供复制功能来实现多个相同数据的Redis副本。

**高可用和分布式**

  Redis从2.8版本正式提供高可用实现的Redis Sentinel，它能保证Redis节点的故障发现与故障自动转移。
  Redis从3.0版本正式提供了分布式实现Redis Cluster。它提供了高可用、读写和容量的扩展。

### Redis的使用场景

**缓存**

**排行榜系统**

**计数器应用**

**社交网络**

**消息队列**

## Redis API

### 全局命令

  ```
  ## 查看所有键
  127.0.0.1:6379> keys *
  (empty list or set)
  127.0.0.1:6379> set java jedis
  OK
  127.0.0.1:6379> keys *
  1) "java"
  ## 键总数
  127.0.0.1:6379> dbsize
  (integer) 1
  ## 键是否存在 存在返回1，不存在返回0
  127.0.0.1:6379> exists a
  (integer) 0
  127.0.0.1:6379> exists java
  (integer) 1
  ## 删除key
  127.0.0.1:6379> del java
  (integer) 1
  ## 键过期   单位秒
  127.0.0.1:6379> expire python 10
  (integer) 1
  ## 查看过期时间 大于0的整数 代表 剩余过期时间  -1 代表未设置 -2 代表已过期（键不存在）
  127.0.0.1:6379> ttl hello
  (integer) -1
  ## 查看key的类型
  127.0.0.1:6379> type python
  none
  127.0.0.1:6379> type hello
  string
  ```

### String 命令

  | 命令 | 说明    | 语法    | 示例|
  |:------:|:----------:|:----------------------|:---------------------------------------------------------------------|
  |set     | 设置值。 ex：秒 px:毫秒 nx: 不存在key才允许set，xx 存在key才允许     |set key value [ex seconds] [px milliseconds] [nx/xx] |set name value px 5000 xx| 
  |setnx   | 设置值,等同于 set key value nx  | setnx key value | setnx key value |
  |setex   | 设置值,等同于 set key value xx  | setex key seconds value | setex name zhangsan value |
  |get     | 获取值,key不存在则返回nil        | get key                 | get name                  |
  |mset    | 批量设置值                      | mset k1 v1 k2 v2 ...    | mset name zhangsan age 32 height 168    |
  |mget    | 批量获取值, key不存在时，返回nil  | mget k1 k2 k3 ...       | mget name age a           |
  |incr    | 自增，值非整数报错，不存在时，插入该key并返回1    | incr key                | incr                      |
  |decr    | 自减                           | decr k1                 | decr count                |
  |incrby  | 自增指定数字                    | incrby key increment    | incrby count 5            |
  |decrby  | 自减指定数字                    | decrby key decrement    | decrby count 5            |
  |incrbyfloat  | 自增指定浮点数             | incrby key increment    | incrby count 5            |  
  |append  | 追加值                         | append key value        | append hello km           |
  |strlen  | 字符串长度                      | strlen key              | str key                   |
  |getset  | 设置并返回原值                  |  getset key             | getset key                |  
  
**典型应用场景**

 缓存、 计数、  限速、 session共享

### 哈希（Hash)

  | 命令 | 说明    | 语法    | 示例|
  |:------:|:----------:|:----------------------|:---------------------------------------------------------------------|
  |hset    | 设置值      |hset key field value   | hset user:1 name tom                        |
  |hget    | 获取值      |hget key field         | hget user:1 name                            |
  |hdel    | 删除field   |hdel key field1 field2 | hdel user:1 name age                        |
  |hlen    | 计算field个数|hlen key              | hlen user:1                                 |
  |hmget   | 批量获取field-value| hmget key field[field...] | --                               |
  |hmset   | 批量设置field-v    | hmset key field value [field value ..] | --                  |
  |hexists | field是否存在|hexists key field     | --                                          |
  |hkeys   | 获取所有的field    | hkeys key      | --                                           |
  |hvals   | 获取所有的value    | hvals key      | --                                           |
  |hgetall | 获取所有的f-v      | hgetall key    | --                                           |

### 列表

  | 操作类     | 操作            |
  |:----------|:--------------- |
  |添加       | rpush lpush linsert |
  |删除       | rpop lpop lrem ltrim|
  |修改       | lset                |
  |查询       | lrange lindex llen  |
  |阻塞操作    | blpop brpop        |
  
  ```
  ## 从右至左加入元素(正常顺序) 
  ## result: a b c d
  127.0.0.1:6379> rpush test_list a b c d
  (integer) 4
  ## 从左至右加入元素(逆序)
  ## result: f e a b c d 
  127.0.0.1:6379> rpush test_list e f
  ## 指定元素前|后 插入元素
  ## result: f nn e a b c d 
  127.0.0.1:6379> linsert test_list BEFORE e nn
  (integer) 10
  ## 从左删除第一个元素
  ## result: nn e a b c d 
  127.0.0.1:6379> lpop test_list
  "f"
  ## 从右删除第一个元素
  ## result: nn e a b c 
  127.0.0.1:6379> rpop test_list
  "d"
  ## 查询当前列表 lrange key start end [] 闭区间 包含 start end
  ## result: nn e a b c  
  127.0.0.1:6379> lrange test_list 0 -1
  ## 查询指定索引下标元素 
  127.0.0.1:6379> lindex test_list 0
  "nn"
  ## 获取列表长度
  127.0.0.1:6379> llen test_list
  (integer) 5
  ## 删除指定元素 lrem key count value 
  ## count>0 从左至右删除 count个元素， count<0 反之，count=0 删除所有
  127.0.0.1:6379> lrem test_list 1 kk
  ## 按照索引范围修剪列表 ltrim key start stop 闭区间 包含start stop 
  127.0.0.1:6379> ltrim test_list 0 2
  OK
  ## 修改列表 lset key index newValue
  127.0.0.1:6379> lset test_list 0 a
  OK
  ```
  
  brpop 与 blpop 是 rpop 和 lpop的阻塞版本。 `brpop key[key...] timeout`
  
  1. 列表为空时，如果timeout=3，则3秒后返回，如果timeout=0，则一直阻塞。
  2. 如果多个key，brpop从左至右遍历key。一旦有值可以pop，立即返回。

**典型应用场景**

  lpush+brpop 实现阻塞式消息队列。

### 集合

  Redis的集合（Set），不光包含简单的增删改查，还支持取交集、并集、差集。合理利用可以解决很多开发中的实际问题。
  
  ```
  ## 添加元素
  127.0.0.1:6379> sadd set_1 a b c
  (integer) 3
  ## 删除元素
  127.0.0.1:6379> srem set_1 a
  (integer) 1
  ## 元素个数
  127.0.0.1:6379> scard set_1
  (integer) 2
  ## 元素是否存在，1 存在 0 不存在
  127.0.0.1:6379> sismember set_1 c
  (integer) 1
  ## 随机取count个元素
  127.0.0.1:6379> srandmember set_1 3
  1) "b"
  2) "c"
  ## 随机弹出元素
  127.0.0.1:6379> spop set_1
  "b"
  ## 获取所有元素
  127.0.0.1:6379> smembers set_1
  ```
  
  集合间的交集、并集与差集
  
  ```
  ## 两个集合
  127.0.0.1:6379> sadd set_1 python java c++
  (integer) 3
  127.0.0.1:6379> sadd set_2 java php .net
  (integer) 3
  ## 交集
  127.0.0.1:6379> sinter set_1 set_2
  1) "java"
  ## 并集
  127.0.0.1:6379> sunion set_1 set_2
  1) "python"
  2) "java"
  3) "php"
  4) "c++"
  5) ".net"
  ## 差集
  127.0.0.1:6379> sdiff set_1 set_2
  1) "python"
  2) "c++"
  ```
  
  **典型应用场景**
  
  标签
  
  ```
  ## 为用户添加标签
  127.0.0.1:6379> sadd user:1:tags 娱乐 社会 电影 科技 体育
  (integer) 5
  127.0.0.1:6379> sadd user:2:tags IT 娱乐 体育
  (integer) 3
  ## 给标签添加用户
  127.0.0.1:6379> sadd tag1:users user:1 user:2
  (integer) 2
  ```

### 有序集合  

  命令
  
  ```
  ## 添加 zadd key [nx | xx] [ch] [incr] score member [score member...]
  ## nx: 不存在才能添加，xx存在才能添加，用于更新 
  ## ch：返回此次操作后，集合元素与分数发生变化的个数
  127.0.0.1:6379> zadd user:ranking 100 tom 99 lisi 88 jack
  (integer) 3
  ## 计算成员个数
  127.0.0.1:6379> zcard user:ranking
  (integer) 4
  ## 获取某成员分数
  127.0.0.1:6379> zscore user:ranking tom
  "101"
  ## 查看成员排名
  127.0.0.1:6379> zrank user:ranking tom
  (integer) 3
  ## 删除成员
  127.0.0.1:6379> zrem user:ranking jack
  ## 返回指定排名范围的成员 
  ## 顺序 zrange key start end [withscores]
  ## 倒序 zrevrange key start end [withscores]
  127.0.0.1:6379> zrange user:ranking 0 3 withscores
  127.0.0.1:6379> zrevrange user:ranking 0 1 withscores
  ## 其他暂时就不写了以后再写吧
  ```
  
  **典型使用场景**
  
  排行榜系统
  