---
layout: page
title: Note of RabbitMQ
---

## RabbitMQ 的模式

详见 [RabbitMQ Tutorials](https://www.rabbitmq.com/getstarted.html)

### Work queues

详见 [Work queues](https://www.rabbitmq.com/tutorials/tutorial-two-python.html)  
多个消费者订阅同一个队列，从而实现高效并发消费消息

### Publish/Subscribe

详见 [Publish/Subscribe](https://www.rabbitmq.com/tutorials/tutorial-three-python.html)  
订阅/发布模式，可以有多个消费者订阅一个队列。生产者向队列生产消息后会推送给所有订阅的消费者

### Routing

详见 [Routing](https://www.rabbitmq.com/tutorials/tutorial-four-python.html)  
路由模式，将一个路由键与队列绑定以后，消费者就可以消费指定的路由键对应的队列

### Topics

详见 [Topics](https://www.rabbitmq.com/tutorials/tutorial-five-python.html)  
主题模式，其实就是路由模式的路由键可以进行规则匹配

### RPC

详见 [RPC](https://www.rabbitmq.com/tutorials/tutorial-six-python.html)  
RPC 模式，听上去屌屌的。实现是利用了两个队列。生产者会在消息里面指定一个响应队列，然后在生产消息以后监听响应队列。消费者消费消息以后会将结果放到响应队列里面去。这个模式可以响应比 ACK 更加复杂的消息