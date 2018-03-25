## kafka是什么？

## kafka原理

## zookeeper在kafka中的作用
1. broker注册
2. topic注册
3. 生产者负载均衡
4. 消费者与分组对应
5. 消费者与分区的对应关系
6. 消费者负载均衡
7. 消费者的offset

## topic和partition
Topic是用于存储消息的逻辑概念，可以看作一个消息集合。每个topic可以有多个生产者向其推送消息，也可以有任意多个消费者消费其中的消息。
每个topic可以划分多个分区（每个Topic至少有一个分区），同一topic下的不同分区包含的消息是不同的。每个消息在被添加到分区时，都会被分配一个offset（称之为偏移量），它是消息在此分区中的唯一编号，kafka通过offset保证消息在分区内的顺序，offset的顺序不跨分区，即kafka只保证在同一个分区内的消息是有序的。


## kafka高吞吐量的原因（高性能）
1. 采用顺序写的方式存储数据。
2. 批量发送（异步发送模式中）
3. 零拷贝

## kafka高可用如何保证？
kafka可靠性依靠的是副本机制。（副本同步队列）副本维护的有资格的follower节点。
1. 副本的所有节点和zookeeper保持连接状态
2. 副本的最后一条消息的offset和leader的最后一条消息的offset不能超过阀值。


## 日志保留策略
无论消费者是否消费了消息，kafka都会保存消息，只是不会长期保存。
1. 根据消息保留的时间，超过保存时间，就可以删除日志。
2. 根据topic存储的数据大小，当topic日志文件大小达到阀值，删除老的日志。

## 日志压缩策略
kafka可以定期针对相同key的消息进行合并，只保留最新的value值。

## 消息可靠性机制
### 如何处理所有的Replica不工作的情况
在ISR中至少有一个follower时，Kafka可以确保已经commit的数据不丢失，但如果某个Partition的所有Replica都宕机了，就无法保证数据不丢失了
1.	等待ISR中的任一个Replica"活"过来，并且选它作为Leader
2.	选择第一个"活"过来的Replica（不一定是ISR中的）作为Leader

如果一定要等待ISR中的Replica"活"过来，那不可用的时间就可能会相对较长。而且如果ISR中的所有Replica都无法"活"过来了，或者数据都丢失了，这个Partition将永远不可用。
选择第一个"活"过来的Replica作为Leader，而这个Replica不是ISR中的Replica，那即使它并不保证已经包含了所有已commit的消息，它也会成为Leader而作为consumer的数据源（前文有说明，所有读写都由Leader完成）。
Kafka0.8.*使用了第二种方式。Kafka支持用户通过配置选择这两种方式中的一种，从而根据不同的使用场景选择高可用性还是强一致性。


## 文件存储方式
在kafka文件存储中，同一个topic下有多个不同的partition，每个partition为一个目录，partition的名称规则为：topic名称+有序序号，第一个序号从0开始，最大的序号为partition数量减1，partition是实际物理上的概念，而topic是逻辑上的概念,partition还可以细分为segment，这个segment是什么呢？ 假设kafka以partition为最小存储单位，那么我们可以想象当kafka producer不断发送消息，必然会引起partition文件的无线扩张，这样对于消息文件的维护以及被消费的消息的清理带来非常大的挑战，所以kafka以segment为单位又把partition进行细分。每个partition相当于一个巨型文件被平均分配到多个大小相等的segment数据文件中（每个setment文件中的消息不一定相等），这种特性方便已经被消费的消息的清理，提高磁盘的利用率。
segment file组成：由2大部分组成，分别为index file和data file，此2个文件一一对应，成对出现，后缀".index"和".log"分别表示为segment索引文件、数据文件.
segment文件命名规则：partion全局的第一个segment从0开始，后续每个segment文件名为上一个segment文件最后一条消息的offset值。数值最大为64位long大小，19位数字字符长度，没有数字用0填充


## 消息确认的方式
1. 自动提交
2. 手动提交
3. 手动异步提交
4. 手动同步提交

## 消息的消费原理

## kafka的分区分配策略
在kafka中每个topic一般都会有很多个partitions。为了提高消息的消费速度，我们可能会启动多个consumer去消费； 同时，kafka存在consumer group的概念，也就是group.id一样的consumer，这些consumer属于一个consumer group，组内的所有消费者协调在一起来消费消费订阅主题的所有分区。当然每一个分区只能由同一个消费组内的consumer来消费，那么同一个consumer group里面的consumer是怎么去分配该消费哪个分区里的数据，这个就设计到了kafka内部分区分配策略（Partition Assignment Strategy）
在 Kafka 内部存在两种默认的分区分配策略：Range（默认） 和 RoundRobin。通过：partition.assignment.strategy指定consumer rebalance
当以下事件发生时，Kafka 将会进行一次分区分配：
1.	同一个consumer group内新增了消费者
2.	消费者离开当前所属的consumer group，包括shuts down 或crashes
3.	订阅的主题新增分区（分区数量发生变化）
4.	消费者主动取消对某个topic的订阅
5.	也就是说，把分区的所有权从一个消费者移到另外一个消费者上，这个是kafka consumer 的rebalance机制。如何rebalance就涉及到前面说的分区分配策略。

### 两种分区策略
Range 策略（默认）
```
0 ，1 ，2 ，3 ，4，5，6，7，8，9
c0 [0,3] c1 [4,6] c2 [7,9]
10(partition num/3(consumer num) =3
```
roundrobin 策略
```
0 ，1 ，2 ，3 ，4，5，6，7，8，9
c0,c1,c2
c0 [0,3,6,9]
c1 [1,4,7]
c2 [2,5,8]
```
kafka 的key 为null， 是随机｛一个Metadata的同步周期内，默认是10分钟｝

