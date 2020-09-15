# RocketMQ

### 组成部分

#### NameServer

路由中心

Namesrv 用于存储 Topic、Broker 关系信息，功能简单，稳定性高

多个 Namesrv 之间相互没有通信，单台 Namesrv 宕机不影响其它 Namesrv 与集群。

多个 Namesrv 之间的信息共享，**通过 Broker 主动向多个 Namesrv 都发起心跳**。正如上文所说，Broker 需要跟所有 Namesrv 连接。

即使整个 Namesrv 集群宕机，已经正常工作的 Producer、Consumer、Broker 仍然能正常工作，但新起的 Producer、Consumer、Broker 就无法工作。

Namesrv 压力不会太大，平时主要开销是在维持心跳和提供 Topic-Broker 的关系数据。但有一点需要注意，Broker 向 Namesr 发心跳时，会带上当前自己所负责的所有 Topic 信息，如果 Topic 个数太多（万级别），会导致一次心跳中，就 Topic 的数据就几十 M，网络情况差的话，网络传输失败，心跳失败，导致 Namesrv 误认为 Broker 心跳失败

#### Broker

#### 高并发读写

- 顺序写
- 随机读(利用PageCache)

#### 负载均衡

Broker 上存 Topic 信息，Topic 由多个队列组成，队列会平均分散在多个 Broker 上，而 Producer 的发送机制保证消息尽量平均分布到所有队列中，最终效果就是所有消息都平均落在每个 Broker 上

#### 动态伸缩

- Topic 维度：假如一个 Topic 的消息量特别大，但集群水位压力还是很低，就可以扩大该 Topic 的队列数， Topic 的队列数跟发送、消费速度成正比

- Broker 维度：如果集群水位很高了，需要扩容，直接加机器部署 Broker 就可以。Broker 启动后向 Namesrv 注册，Producer、Consumer 通过 Namesrv 发现新Broker，立即跟该 Broker 直连，收发消息。

#### Producer

- 启动时，先跟 Namesrv 集群中的其中一台建立长连接，并从Namesrv 中获取当前发送的 Topic 存在哪些 Broker 上，然后跟对应的 Broker 建立长连接，直接向 Broker 发消息。

- 生产者每 30 秒从 Namesrv 获取 Topic 跟 Broker 的映射关系，更新到本地内存中。然后再跟 Topic 涉及的所有 Broker 建立长连接，每隔 30 秒发一次心跳。
- 在 Broker 端也会每 10 秒扫描一次当前注册的 Producer ，如果发现某个 Producer 超过 2 
- 分钟都没有发心跳，则断开连接。

##### 消息发送方式

1. 同步方式
2. 异步方式
3. Oneway 方式(不需要Broker返回结果)

#### Consumer

- Consumer 启动时需要指定 Namesrv 地址，与其中一个 Namesrv 建立长连接。消费者每隔 30 秒从 Namesrv 获取所有Topic 的最新队列情况，这意味着某个 Broker 如果宕机，客户端最多要 30 秒才能感知。连接建立后，从 Namesrv 中获取当前消费 Topic 所涉及的 Broker，直连 Broker 。
- Consumer 跟 Broker 是长连接，会每隔 30 秒发心跳信息到Broker 。Broker 端每 10 秒检查一次当前存活的 Consumer ，若发现某个 Consumer 2 分钟内没有心跳，就断开与该 Consumer 的连接，并且向该消费组的其他实例发送通知，触发该消费者集群的负载均衡。



### 消费模式

#### 1.集群模式

- 消息进度持久化到Broker

- 每个消费者消费特定Queue

#### 2.广播模式

- 每个Consumer消费所有Queue

- 消费进度持久化本地



### 顺序消息

[RocketMQ原理图](https://github.com/yafeites/rocketmqLearning/blob/master/RocketMQ%E5%8E%9F%E7%90%86%E5%9B%BE.md)

### 延时消息

[RocketMQ原理图](https://github.com/yafeites/rocketmqLearning/blob/master/RocketMQ%E5%8E%9F%E7%90%86%E5%9B%BE.md)

### 事务消息

[事务消息](https://github.com/yafeites/rocketmqLearning/blob/master/rocketMQ%E5%AD%A6%E4%B9%A0%E6%80%BB%E7%BB%93/RocketMQ%20%E4%BA%8B%E5%8A%A1%E6%B6%88%E6%81%AF%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86.md)



### 如何实现消息不丢？

#### 消费端

消费成功后异步更新进度到Broker 并且本地存储结果

消费失败会将消息重新发送到Broker

#### Broker端

同步刷盘+同步复制保证消息不丢

#### 生产端

Producer 可以设置三次发送消息重试。



### 消息幂等性

Producer发送消息时,MQ默认生成消息编号全局唯一,保证消息唯一

业务方自己保证消息幂等