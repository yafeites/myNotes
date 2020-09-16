# Zookeeper

### 什么是Zookeeper？

ZooKeeper主要**服务于分布式系统**，可以用ZooKeeper来做：统一配置管理、统一命名服务、分布式锁、集群管理

### 特性

- ##### 顺序一致性(有序性)

从同一个客户端发起的事务请求，最终将会严格地按照其发起顺序被应用到 Zookeeper 中去。

> 从同一个客户端发起的事务请求，最终将会严格地按照其发起顺序被应用到 Zookeeper 中去。
>
> 有序性是 Zookeeper 中非常重要的一个特性。
>
> - 所有的更新都是全局有序的，每个更新都有一个唯一的时间戳，这个时间戳称为zxid(Zookeeper Transaction Id)。
> - 而读请求只会相对于更新有序，也就是读请求的返回结果中会带有这个 Zookeeper 最新的 zxid 。

- ##### 原子性

> 所有事务请求的处理结果在整个集群中所有机器上的应用情况是一致的，即整个集群要么都成功应用了某个事务，要么都没有应用。

- ##### 单一视图

> 无论客户端连接的是哪个 Zookeeper 服务器，其看到的服务端数据模型都是一致的



- ##### 可靠性

一旦服务端成功地应用了一个事务，并完成对客户端的响应，那么该事务所引起的服务端状态变更将会一直被保留，除非有另一个事务对其进行了变更。

- ##### 实时性

  > Zookeeper 保证在一定的时间段内，客户端最终一定能够从服务端上读取到最新的数据状态。



### Zookeeper的应用场景



- 统一命名服务。

  > 命名服务是指通过指定的名字来获取资源或服务的地址，利用zk创建一个全局的路径，即时唯一的路径，这个路径就可以作为一个名字，指向集群中机器或者提供服务的地址，又或者一个远程的对象等。

  

- 分布式锁服务。

> 有了 ZooKeeper 的一致性文件系统，锁的问题变得容易。锁服务可以分成两类，一个是保持独占，另一个是控制时序。
>
> - 1、保持独占，我们把 znode 看作是一把锁，通过 createZnode 的方式来实现。所有客户端都去创建 `/distribute_lock` 节点，最终成功创建的那个客户端也即拥有了这把锁。用完删除掉自己创建的 `/distribute_lock` 节点就释放出锁。
> - 2、控制时序，`/distribute_lock` 已经预先存在，所有客户端在它下面创建临时顺序编号目录节点，和 Master 一样，编号最小的获得锁，用完删除，依次方便。



- 配置管理。

> 例如说，[Spring Cloud Config Zookeeper](https://blog.csdn.net/CSDN_Stephen/article/details/78856323) ，就实现了基于 Zookeeper 的 Spring Cloud Config 的实现，提供配置中心的服务。



- 注册与发现。

> 是否有机器加入或退出
>
> 所有机器约定在父目录下创建临时目录节点，然后监听父目录节点下的子节点变化。一旦有机器挂掉，该机器与 ZooKeeper 的连接断开，其所创建的临时目录节点也被删除，所有其他机器都收到通知：某个节点被删除了。

- Master 选举。

> 基于 Zookeeper 实现分布式协调，从而实现主从的选举。这个在 Kafka、Elastic-Job 等等中间件，都有所使用到。



### Zookeeper文件系统

Zookeeper 提供一个多层级的节点命名空间(节点称为 znode)。与文件系统不同的是，这些节点都可以设置关联的数据，而文件系统中只有文件节点可以存放数据而目录节点不行。

Zookeeper 为了保证高吞吐和低延迟，在内存中维护了这个树状的目录结构，这种特性使得 Zookeeper 不能用于存放大量的数据，每个节点的存放数据上限为 1M



### Zookeeper 有哪几种节点类型？

- PERSISTENT 持久节点

- PERSISTENT_SEQUENTIAL 持久顺序节点

- EPHEMERAL 临时节点

- EPHEMERAL_SEQUENTIAL 临时顺序节点

### Zookeeper通知机制

- 第一步，客户端注册 Watcher 。
- 第二步，服务端处理 Watcher 。
- 第三步，客户端回调 Watcher 。

#### Watcher 的特性总结：

1、一次性。

2、客户端串行执行。

> 客户端 Watcher 回调的过程是一个串行同步的过程

3、轻量级 Watch 机制。

> Watcher 通知非常简单，只会告诉客户端发生了事件，而不会说明事件的具体内容。
>
> 客户端向服务端注册 Watcher 的时候，并不会把客户端真实的 Watcher 对象实体传递到服务端，仅仅是在客户端请求中使用`boolean` 类型属性进行了标记。

4、Watcher event 异步发送 Watcher 的通知事件从 Server 发送到Client 是异步的，这就存在一个问题，不同的客户端和服务器之间通过Socket 进行通信，由于网络延迟或其他因素导致客户端在不通的时刻监听到事件，由于 Zookeeper 本身提供了 ordering guarantee ，即客户端监听事件后，才会感知它所监视 znode 发生了变化。所以我们使用 Zookeeper 不能期望能够监控到节点每次的变化。**Zookeeper 只能保证最终的一致性，而无法保证强一致性**

5、可以注册 Watcher 的操作：getData、exists、getChildren 。

6、可以触发 Watcher 的操作：create、delete、setData 。

7、当一个 Client 连接到一个新的服务器上时，watch 将会被以任意会话事件触发。当与一个服务器失去连接的时候，是无法接收到 watch 的。而当 Client 重新连接时，如果需要的话，所有先前注册过的watch ，都会被重新注册。通常这是完全透明的。只有在一个特殊情况下，watch 可能会丢失：对于一个未创建的 znode 的 exists watch ，如果在客户端断开连接期间被创建了，并且随后在客户端连接上之前又删除了，这种情况下，这个 watch 事件可能会被丢失。

### Zookeeper 的部署方式？

- 1、单机
- 2、集群

> Zookeeper 集群，是一个由多个 Server 组成，一个 Leader，多个 Follower。（这个不同于我们常见的 Master/Slave 模式）Leader 为客户端服务器提供读写服务，除了 Leader 外其他的机器只能提供读服务。
>
> 每个 Server 保存一份数据副本全数据一致，分布式读 Follower ，写由 Leader 实施更新请求转发，由 Leader 实施更新请求顺序进行，来自同一个 Client 的更新请求按其发送顺序依次执行数据更新原子性，一次数据更新要么成功，要么失败。
>
> 全局唯一数据视图，Client 无论连接到哪个 Server，数据视图都是一致的实时性，在一定事件范围内，Client 能读到最新数据。

集群中一共有三种角色：

- 1、Leader

  > - 事务请求的唯一调度和处理者，保证集群事务处理的顺序性。
  > - 集群内部各服务的调度者。

- 2、Follower

  > - 处理客户端的非事务请求，转发事务请求给 Leader 服务器。
  > - 参与事务请求 Proposal 的投票。
  > - 参与 Leader 选举投票。

- 3、Observer

> 3.3.0 版本以后引入的一个服务器角色，在不影响集群事务处理能力的基础上提升集群的非事务处理能力。
>
> - 处理客户端的非事务请求，转发事务请求给 Leader 服务器
> - 不参与任何形式的投票。
>
> 如果 ZooKeeper 集群的读取负载很高，或者客户端多到跨机房，可以设置一些 Observer 服务器，以提高读取的吞吐量。Observer 和 Follower 比较相似，只有一些小区别：
>
> - 首先 Observer 不属于法定人数，即不参加选举也不响应提议，也不参与写操作的“过半写成功”策略；
> - 其次是 Observer 不需要将事务持久化到磁盘，一旦 Observer 被重启，需要从 Leader 重新同步整个名字空间。



### ZooKeeper 的工作原理？

ZooKeeper 的核心是原子广播，这个机制保证了各个 Server 之间的同步。实现这个机制的协议叫做 **Zab** 协议。Zab 协议有两种模式，它们分别是恢复模式（选主）和广播模式（同步）：

- 选主：当服务启动或者 Leader 崩溃后，Zab 就进入了恢复模式，当新的 Leader 被选举出来，且大多数 Server 完成了和 Leader 的状态同步以后，恢复模式就结束了。

> 更加详细的描述。
>
> 当整个 Zookeeper 集群刚刚启动，或者 Leader 服务器宕机、重启或者网络故障导致不存在过半的服务器与 Leader服务器保持正常通信时，所有进程（服务器）进入崩溃恢复模式。
>
> - 首先，选举产生新的Leader服务器。
> - 然后，集群中 Follower 服务器开始与新的 Leader 服务器进行数据同步。
> - 当集群中超过半数机器与该Leader服务器完成数据同步之后，退出恢复模式进入消息广播模式



同步：状态同步保证了 Leader 和 Server 具有相同的系统状态。

> 更加详细的描述。
>
> Leader 服务器开始接收客户端的事务请求，生成事务提案来进行事务请求处理。



#### ZooKeeper 是如何保证事务的顺序一致性的？



ZooKeeper 采用了递增的事务 id 来识别，所有的 proposal（提议）都在被提出的时候加上了 zxid 。zxid 实际上是一个 64 位数字。

- 高 32 位是 epoch 用来标识 Leader 是否发生了改变，如果有新的 Leader 产生出来，epoch会自增。
- 低 32 位用来递增计数。

当新产生的 peoposal 的时候，会依据数据库的两阶段过程，首先会向其他的 Server 发出事务执行请求，如果超过半数的机器都能执行并且能够成功，那么就会开始执行。

#### **ZooKeeper 集群中个服务器之间是怎样通信的？**

Leader 服务器会和每一个 Follower/Observer 服务器都建立 TCP 连接，同时为每个 Follower/Observer 都创建一个叫做 LearnerHandler 的实体。

- LearnerHandler 主要负责 Leader 和 Follower/Observer 之间的网络通讯，包括数据同步，请求转发和 Proposal 提议的投票等。
- Leader 服务器保存了所有 Follower/Observer 的 LearnerHandler 。

### Zookeeper 的选举过程

#### **Zookeeper 选主流程(fast paxos)**

##### FastLeaderElection选举算法

FastLeaderElection是标准的Fast Paxos的实现，它首先向所有Server提议自己要成为leader，当其它Server收到提议以后，解决 epoch 和 zxid 的冲突，并接受对方的提议，然后向对方发送接受提议完成的消息。FastLeaderElection算法通过异步的通信方式来收集其它节点的选票，同时在分析选票时又根据投票者的当前状态来作不同的处理，以加快Leader的选举进程




#### **数据恢复阶段**

每个ZooKeeper Server读取当前磁盘的数据（transaction log），获取最大的zxid。

**发送选票**

每个参与投票的ZooKeeper Server向其他Server发送自己所推荐的Leader，这个协议中包括几部分数据：

- 所推举的Leader id。在初始阶段，第一次投票所有Server都推举自己为Leader。
- 本机的最大zxid值。这个值越大，说明该Server的数据越新。
- logicalclock。这个值从0开始递增，每次选举对应一个值，即在同一次选举中，这个值是一致的。这个值越大说明选举进程越新。
- 本机的所处状态。包括LOOKING，FOLLOWING，OBSERVING，LEADING。



**处理选票**

每台Server将自己的数据发送给其他Server之后，同样也要接受其他Server的选票，并做一下处理。

**如果Sender的状态是LOOKING**

- 如果发送过来的logicalclock大于目前的logicalclock。说明这是更新的一次选举，需要更新本机的logicalclock，同时清空已经收集到的选票，因为这些数据已经不再有效。然后判断是否需要更新自己的选举情况。首先判断zxid，zxid大者胜出；如果相同比较leader id，大者胜出。
- 如果发送过来的logicalclock小于于目前的logicalclock。说明对方处于一个比较早的选举进程，只需要将本机的数据发送过去即可。
- 如果发送过来的logicalclock等于目前的logicalclock。根据收到的zxid和leader id更新选票，然后广播出去。

当Server处理完选票后，可能需要对Server的状态进行更新：

- 判断服务器是否已经收集到所有的服务器的选举状态。如果是根据选举结果设置自己的角色（FOLLOWING or LEADER），然后退出选举。
- 如果没有收到没有所有服务器的选举状态，也可以判断一下根据以上过程之后更新的选举Leader是不是得到了超过半数以上服务器的支持。如果是，那么尝试在200ms内接收下数据，如果没有心数据到来说明大家已经认同这个结果。这时，设置角色然后退出选举。

#### 如果Sender的状态是FOLLOWING或者LEADER

- 如果LogicalClock相同，将数据保存早recvset，如果Sender宣称自己是Leader，那么判断是不是半数以上的服务器都选举它，如果是设置角色并退出选举。
- 否则，这是一条与当前LogicalClock不符合的消息，说明在另一个选举过程中已经有了选举结果，于是将该选举结果加入到OutOfElection集合中，根据OutOfElection来判断是否可以结束选举，如果可以也是保存LogicalClock，更新角色，退出选举。


![fast-leader-election](Zookeeper.assets/165bc888045b6de1)

#### 广播(broadcast)模式

1. leader从客户端收到一个写请求
2. leader生成一个新的事务并为这个事务生成一个唯一的ZXID，
3. leader将这个事务发送给所有的follows节点
4. follower节点将收到的事务请求加入到历史队列(history queue)中,并发送ack给ack给leader
5. 当leader收到大多数follower（超过法定数量）的ack消息，leader会发送commit请求
6. 当follower收到commit请求时，会判断该事务的ZXID是不是比历史队列中的任何事务的ZXID都大，如果是则提交，如果不是则等待比它更大的事务的commit