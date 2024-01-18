# 消息队列的介绍
## 消息队列的概念
消息队列是在消息的传输过程中保存消息的容器


## 消息队列和jms
在计算机科学中，消息队列（英语：Message queue）是一种进程间通信或同一进程的不同线程间的通信方式，软件的贮列用来处理一系列的输入，通常是来自用户。消息队列提供了异步的通信协议，每一个队列中的纪录包含详细说明的数据，包含发生的时间，输入设备的种类，以及特定的输入参数，也就是说：消息的发送者和接收者不需要同时与消息队列交互。消息会保存在队列中，直到接收者取回它。

Java消息服务（Java Message Service，JMS）应用程序接口是一个Java平台中关于面向消息中间件（MOM）的API，用于在两个应用程序之间，或分布式系统中发送消息，进行异步通信。Java消息服务是一个与具体平台无关的API，绝大多数MOM提供商都对JMS提供支持。

## mq的作用和使用场景
**1、异步化、**

* 1）业务系统触发短信发送申请，但短信发送模块速度跟不上，需要将来不及处理的消息暂存一下，缓冲压力。就可以把短信发送申请丢到消息队列，直接返回用户成功，短信发送模块再可以慢慢去消息队列中取消息进行处理。

* 2）支付场景，调用第三方接口支付，支付成功后收到对方的消息

* 3）任务处理类的系统，先把用户发起的任务请求接收过来存到消息队列中，然后后端开启多个应用程序从队列中取任务进行处理。

  

**2、解耦、**

* 业务系统和发送短信系统解耦，不需要直接rpc调用

  

**3、消除峰值**

* 促销或者活动，可以把请求发到消息队列，然后转发到服务，避免服务直接挂掉。



## mq的优缺点

* **优点：**
  * 1、系统解构，提升服务扩展性，实现系统的高扩展性
  * 2、异步化处理，提升服务性能，实现系统的高性能
  * 3、消除峰值，提升服务稳定性，实现系统的高可用性
* **缺点**
  * 系统更复杂更高
  * 消息传递路径增加，延时增加
  * 消息可靠性和重复性难以同时保证
  * 上游无法知道下游的执行结果

## 什么情况使用mq，rpc
**使用mq：**

* 上游不关心多个下游的执行结果
* 不需要同步执行
* 异步回调时间长

使用用rpc：

* 上游关注执行结果
* 需要进行数据交互
* 需要同步执行，有依赖



## 常见消息队列

rabbitMq,zeroMq,rocketMq,kafaka



## 消息队列模型
- pull和push，
- 点对点，
- 广播，
- 流式处理



## rabbitmq和kafka的区别

功能性：

实现原理：



## 如何保证消息的可靠性

如果说你这个是用 MQ 来传递非常核心的消息，比如说计费、扣费的一些消息，那必须确保这个 MQ 传递过程中绝对不会把计费消息给弄丢。



## 如何保证消息不重复
