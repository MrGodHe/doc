# AMQP 协议 
    Advanced Message Queuing Protocol 高级消息队列协议

# 什么是RabbitMQ ？ 原理结构？
```
实现了高级消息队列协议（AMQP）的开源消息代理软件（消息中间件）
```

![](https://github.com/MrGodHe/doc/blob/master/image/rabbitMq/rabbitMq.png)

 **组成机构**

1. **Producer**：消息的生产者

2. **Comsumer**：消息的消费者

3. **RabbitMQ Server**： 消息队列实体服务器，又称Broker

  由以下组成：

  **虚拟主机 （vhost）**
      每一个RabbitMQ服务器都能创建虚拟消息服务器，mini版的RabbitMQ（开箱即用的默认的虚拟主机“/”）
  **交换机，队列绑定（ Exchange & Bindings）**

  - **direct**：处理路由键，binding key 与 routing key完全匹配	

    ![](https://github.com/MrGodHe/doc/blob/master/image/rabbitMq/rabbitMq_direct.png)

  - **fanout**：不处理路由键（广播），它会把所有发送到该Exchage的消息路由到所有与它绑定的Queue中

    ![](https://github.com/MrGodHe/doc/blob/master/image/rabbitMq/rabbitMq_fanout.png)

  - **topic ：**匹配路由键， binding key 与 routing key 匹配（一对多的模糊匹配）

    ![](https://github.com/MrGodHe/doc/blob/master/image/rabbitMq/rabbitMq_topic.png)

  - **headers：**根据发送的消息内容中的headers属性进行匹配

  **队列、消息的存储 （Queue）**
  	1、持久性（消息代理重启后，队列依旧存在）
  	2、独享（只被一个连接（connection）使用，而且当连接关闭后队列即被删除）
  	3、自动删除（当最后一个消费者退订后即被删除）
  	4、其他参数：（消息代理用他来完成类似与TTL的某些额外功能）  

# 消息持久化

- **队列持久化**

  方式：队列的持久化是在声明队列时，将durable设置为true实现的。
  解决问题：若队列不设置持久化，那么当MQ服务重启时，队列和队列的数据会丢失

- **消息持久化**

  方式：消息的持久化，需要通过将消息的投递模式BasicProperties中的deliveryMode属性设置为2
  解决问题：防止MQ服务重启时，消息会丢失

# 消费消息的可靠性
    1、消费者交货确认（broker -> 消费者）
    2、发布者确认（生产者 -> broker）
        在Channel上启用发布者确认
        Channel channel = connection.createChannel（）;
        channel.confirmSelect（）;
        策略一：即发布消息并同步等待确认。
                通过 waitForConfirmsOrDie（5_000）//使用5秒超时，该方法进行确认，如果未在超时时间内确认消息，或者该消息没被
                确认，则将产生异常，异常的处理通常包括记录错误消息和/或重试发送消息。
                （注：不同的客户端库有不同的方式来同步处理发布者的确认，因此请确保仔细阅读所使用客户端的文档）
                缺点：由于消息的确认会阻止所有后续消息的发布，因此它会大大减慢发布速度
                
        策略二：批量发布消息，并等待整个批次被确认。
        策略三：代理异步确认已发布的消息，只需在客户端上注册一个回调即可收到这些确认的通知。
               
               有两种回调：
               第一种用于确认消息；
               第二种用于小消息（broker可以认为丢失的消息）；
               
               每个回调都有2个参数：
               序列号：标识已确认或未确认消息的数字。我们很快将看到如何将其与已发布的消息相关联。
                     multiple：这是一个布尔值。如果为false，则仅确认/取消一条消息；如果为true，
                               则将确认/不添加序列号较低或相等的所有消息。
                Channel channel = connection.createChannel();
                channel.confirmSelect();
                channel.addConfirmListener((sequenceNumber, multiple) -> {
                    // code when message is confirmed
                }, (sequenceNumber, multiple) -> {
                    // code when message is nack-ed
                });

