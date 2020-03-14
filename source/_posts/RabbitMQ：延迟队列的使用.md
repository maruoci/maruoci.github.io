---
title: RabbitMQ：延时队列的实现
date: 2020-03-11 16:47:42
categories:
- RabbitMQ
tags:
- RabbitMQ
---

 本文主要从以下几个方面开始：

 ![](https://raw.githubusercontent.com/maruoci/images/master/Java/RabbitMQ/mq-delay-00.jpg)

## 延迟队列的理解

延迟队列，顾名思义就是生产的消息在指定时间进行消费。

比如：

+ 12306买票，30分钟内未付款订单自动取消。
+ 注册某理财平台后，特定时间没有出借资金，会有短信来提醒。

<!-- more -->

## 理解TTL 与 DLX

### TTL

TTL，Time-To-Live的缩写，就是消息的生存时间。

RabbitMQ中支持消息和队列设置TTL。

**通过界面设置TTL**

1. 创建Exchange

 选择Exchange菜单，“add a new exchange”

 ![](https://raw.githubusercontent.com/maruoci/images/master/Java/RabbitMQ/mq-delay-01.jpg)

 ```
 exchange的属性：
   1. name：名称
   2. Durability：持久化标志，如果为true，表示exchange是持久化。
   3. Auto-delete：删除标志，当所有队列在完成使用此exchange时，是否删除
   4. Arguments:参数
 exchange的类型：
   1. Direct: 消息与一个特定的路由键完全匹配
   2. Fanout: 不处理路由，发送到该类型交换机的消息都会被广播到与该交换机绑定的所有队列上。
   3. Topic：路由与某模式匹配。
   4. Headers：不处理路由键，而是根据发送的消息内容中的headers属性进行匹配。

 ```

2. 创建Queue

 选择Queue，“add a new queue”，这里队列设置了10秒的过期时间。

 ![](https://raw.githubusercontent.com/maruoci/images/master/Java/RabbitMQ/mq-delay-02.jpg)

3. 建立队列与交换机的绑定

 选择Exchange，选中刚建立的exchange，和queue绑定。

 ![](https://raw.githubusercontent.com/maruoci/images/master/Java/RabbitMQ/mq-delay-03.jpg)

4. 发送消息

 点击Exchange表格中的test-exchange-ddl，在下面找到Publish message，设置消息进行发送

 ![](https://raw.githubusercontent.com/maruoci/images/master/Java/RabbitMQ/mq-delay-04.jpg)

5. 验证

 ![](https://raw.githubusercontent.com/maruoci/images/master/Java/RabbitMQ/mq-delay-05.jpg)

 因为上面的队列TTL设置为10秒，所以10秒后，消息自动清除。

6. 设置单条消息过期时间

 ![](https://raw.githubusercontent.com/maruoci/images/master/Java/RabbitMQ/mq-delay-06.jpg)

 消息和队列同时设置过期时间时，以最小值为准。

**代码设置TTL**

```
 /**
  * 队列设置过期时间
  */
@Bean
public Queue setQueueTTL() {
    // 设置队列消息过期时间
    Map<String, Object> arguments = new HashMap<>();
    arguments.put("x-message-ttl", 5000);
    return new Queue("queue-name", true, false, false, arguments);
}
/**
 * 设置消息过期时间
 */
public void setMsgTTL(){
    rabbitTemplate.convertAndSend("routing-key", "msg_content", message -> {
        MessageProperties messageProperties = message.getMessageProperties();
        // 设置这条消息的过期时间
        messageProperties.setExpiration("5000");
        return message;
    });
}
```

### DLX

 DLX, DEAD-LINE-EXCHANGE,直译过来就是死信交换机。当出现死信时，通过DLX将死信发送到死信队列。

 这里对几个概念进行描述：

 + 死信：当出现以下几种情况时，消息就被称为死信。

 > 1. 消息被拒绝(Basic.Reject或Basic.Nack)，并且requeue为false。
 > 2. 消息TTL过期。
 > 3. 队列达到最大长度。

 + 死信交换机： DLX，出现死信时，消息就会发送到死信交换机。

 > Q: 如何定义个交换机为死信交换机呢，死信如何被死信队列消费？

 + 死信队列：绑定DLX的队列就是死信队列。

 上面我们通过界面发送消息，消息过期后，因为没有配置DLX，消息被丢弃。

 **如何配置DLX**

 简单说来就以下三步：

 1. 配置业务队列，绑定到业务交换机上。
 2. 为业务队列配置死信交换机和死信路由key。
 3. 为死信交换机配置死信队列。

 **Demo**

 此处的示例是SpringBoot集成的。
 
 pom依赖
 
 ```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-amqp</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
 ```

 配置业务队列+业务交换机，绑定路由

 配置死信队列+死信交换机，绑定路由
 
 ```
package com.example.deadletter;

import org.springframework.amqp.core.*;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import java.util.HashMap;
import java.util.Map;

@Configuration
public class DLXConfig {
    public static final String BUSI_EXCHANGE = "busi.exchange";
    public static final String BUSI_QUEUE = "busi.queue";
    public static final String BUSI_ROUTING_KEY = "busi.routing-key";
    public static final String DLX_EXCHANGE = "dlx.exchange";
    public static final String DLX_ROUTING_KEY = "dlx.routing-key";
    public static final String DLX_QUEUE = "dlx.queue";
    /**
     * 1 定义业务队列交换机
     */
    @Bean
    public DirectExchange busiExchange() {
        return new DirectExchange(BUSI_EXCHANGE);
    }
    /**
     * 2. 声明业务队列（设定过期时间，时间到达，消息变为死信）
     * a. 设定 死信交换机 和 路由key
     */
    @Bean
    public Queue busiQueue() {
        Map<String, Object> args = new HashMap<>(2);
        args.put("x-dead-letter-exchange", DLX_EXCHANGE);
        args.put("x-dead-letter-routing-key", DLX_ROUTING_KEY);
        args.put("x-message-ttl", 5000);
        return QueueBuilder.durable(BUSI_QUEUE).withArguments(args).build();
    }
    /**
     * 定义DLX 和 死信队列
     *
     * @return
     */
    @Bean
    public DirectExchange dlxExchange() {
        return new DirectExchange(DLX_EXCHANGE);
    }
    @Bean
    public Queue dlxQueue() {
        return QueueBuilder.durable(DLX_QUEUE).build();
    }
    //建立绑定关系
    @Bean
    public Binding busiBinding() {
        return BindingBuilder.bind(busiQueue()).to(busiExchange()).with(BUSI_ROUTING_KEY);
    }
    @Bean
    public Binding dlxBinding() {
        return BindingBuilder.bind(dlxQueue()).to(dlxExchange()).with(DLX_ROUTING_KEY);
    }
}
 ```

 死信队列监听器

 ```
 @Component
 public class DlxListener {

    @RabbitListener(queues = DLXConfig.DLX_QUEUE)
    public void receive(Message msg) {
        System.out.println(LocalDateTime.now() + ": 监听到死信队列消息：" + msg);
    }
 }
 ```

 模拟消息发送

 ```
 @RestController
 public class MsgProducer {

    @Autowired
    RabbitTemplate rabbitTemplate;

    @GetMapping("test")
    public void test(String name, String age) {
        Map<String, String> msg = new HashMap<>();
        msg.put("name", name);
        msg.put("age", age);
        rabbitTemplate.convertAndSend(DLXConfig.BUSI_EXCHANGE, DLXConfig.BUSI_ROUTING_KEY, msg);
        System.out.println(LocalDateTime.now() + "发送业务消息:" + msg);
    }
 }
 ```

 启动SpringBoot后，访问localhost:8080/test?name=mark&age=24,查看控制台可看到消息进入死信队列被消费。

 ![](https://raw.githubusercontent.com/maruoci/images/master/Java/RabbitMQ/mq-delay-07.jpg)

 **[示例代码](https://github.com/maruoci/spring-boot-example/tree/master/spring-boot-rabbitmq)**

 上面的两个问题到这里也基本有了答案：

 > Q: 如何定义个交换机为死信交换机呢，死信如何被死信队列消费？
 > A: 死信交换机其实就是一个普通的exchange，业务消息队列在设置时会设置DLX和死信路由，死信根据DLX和路由进入死信队列。

## 如何利用RabbitMQ实现延迟队列

 看明白上面的死信Demo，其实就明白了延迟队列的实现方式。我们梳理一下：

 1. 设定一个过渡队列A，路由A和交换机A，队列A中设置TTL。
 2. 设定一个死信队列B，该死信即为延迟消费的业务队列。
 3. 队列A设定死信路由及死信交换机DLX。
 4. 绑定。

## 总结

 1. TTL过期时间，可设置在队列和消息，如果都设置的话，取最小值。
 2. 当消息过期，如果队列设置DLX，会进入死信交换机。（死信交换机就是一个普通的交换机）

## 参考
 
 + [RabbitMQ的TTL消息详解](https://www.jianshu.com/p/d7eb12fec784)
 + [【RabbitMQ】一文带你搞定RabbitMQ死信队列](https://cloud.tencent.com/developer/article/1463065)


