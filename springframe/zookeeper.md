---
title: zookeeper
tags: 注册中心，zookeeper，java
grammar_cjkRuby: true
---

## 功能简介
 ZooKeeper 是一个开源的分布式协调服务，由雅虎创建，是 Google Chubby 的开源实现。分布式应用程序可以基于 ZooKeeper 实现诸如数据发布/订阅、负载均衡、命名服务、分布式协调/通知、集群管理、Master 选举、配置维护，名字服务、分布式同步、分布式锁和分布式队列等功能。
 
## ZooKeeper基本概念


### 集群角色
&emsp;&emsp;zookeeper中包含三个角色：Leader、Follower、Observer

&emsp;&emsp;一个 ZooKeeper 集群同一时刻只会有一个 Leader，其他都是 Follower 或 Observer。

&emsp;&emsp;ZooKeeper 配置很简单，每个节点的配置文件(zoo.cfg)都是一样的，只有 myid 文件不一样。myid 的值必须是 zoo.cfg中server.{数值} 的{数值}部分。

&emsp;&emsp;zoo.cfg配置文件示例 

![zoo.cfg](./images/zookeeper.jpg)

&emsp;&emsp;在装有 ZooKeeper 的机器的终端执行 zookeeper-server status 可以看当前节点的 ZooKeeper是什么角色（Leader or Follower）。

&emsp;&emsp;ZooKeeper 默认只有 Leader 和 Follower 两种角色，没有 Observer 角色。为了使用 Observer 模式，在任何想变成Observer的节点的配置文件中加入:peerType=observer 并在所有 server 的配置文件中，配置成 observer 模式的 server 的那行配置追加 :observer

### 节点读写服务分工

&emsp;&emsp;1.ZooKeeper `集群的所有机器通过一个 Leader 选举过程来选定一台被称为『Leader』的机器`，`Leader服务器为客户端提供读和写服务。`

&emsp;&emsp;2.Follower 和 Observer 都能提供读服务，不能提供写服务。两者唯一的区别在于，`Observer机器不参与 Leader 选举过程，也不参与写操作的『过半写成功』策略`，因此 `Observer 可以在不影响写性能的情况下提升集群的读性能。`

### Session
&emsp;&emsp;Session 是指客户端会话，在讲解客户端会话之前，我们先来了解下客户端连接。`在ZooKeeper 中，一个客户端连接是指客户端和 ZooKeeper 服务器之间的TCP长连接。`
&emsp;&emsp;ZooKeeper 对外的服务端口默认是2181，客户端启动时，首先会与服务器`建立一个TCP连接`，从第一次连接建立开始`客户端会话的生命周期也开始了`，通过这个连接，客户端`能够通过心跳检测和服务器保持有效的会话`，也能够向 ZooKeeper 服务器发送请求并接受响应，同时还能通过该连接接收来自服务器的 Watch 事件通知。
&emsp;&emsp;Session 的 SessionTimeout 值用来设置一个客户端会话的超时时间。当由于服务器压力太大、网络故障或是客户端主动断开连接等各种原因导致客户端连接断开时，`只要在SessionTimeout 规定的时间内能够重新连接上集群中任意一台服务器，那么之前创建的会话仍然有效`。

### 数据节点

&emsp;&emsp;zookeeper的结构其实就是一个树形结构，leader就相当于其中的根结点，其它节点就相当follow节点，每个节点都保留自己的内容。
&emsp;&emsp;zookeeper的节点分两类：持久节点和临时节点
&emsp;&emsp;- 持久节点：
&emsp;&emsp;&emsp;&emsp;所谓持久节点是指一旦这个树形结构上被创建了，除非主动进行对树节点的移除操作，否则这个节点将一直保存在 ZooKeeper 上。
&emsp;&emsp;-临时节点：
&emsp;&emsp;&emsp;&emsp;临时节点的生命周期跟客户端会话绑定，一旦客户端会话失效，那么这个客户端创建的所有临时节点都会被移除。

### 状态信息

&emsp;&emsp;每个节点除了存储数据内容之外，还存储了 节点本身的一些状态信息。用 get 命令可以同时获得某个 节点的内容和状态信息
&emsp;&emsp;在 ZooKeeper 中，version 属性是用来实现乐观锁机制中的『写入校验』的（保证分布式数据原子性操作）。

### 事物操作

&emsp;&emsp;在ZooKeeper中，能改变ZooKeeper服务器状态的操作称为事务操作。一般包括数据节点创建与删除、数据内容更新和客户端会话创建与失效等操作。对应每一个事务请求，ZooKeeper都会为其分配一个全局唯一的事务ID，用 ZXID 表示，通常是一个64位的数字。每一个ZXID对应一次更新操作，从这些 ZXID 中可以间接地识别出 ZooKeeper 处理这些事务操作请求的全局顺序。

### Watcher(事件监听器)

&emsp;&emsp;是 ZooKeeper 中一个很重要的特性。ZooKeeper允许用户在指定节点上注册一些 Watcher，并且在一些特定事件触发的时候，ZooKeeper 服务端会将事件通知到感兴趣的客户端上去。该机制是 ZooKeeper 实现分布式协调服务的重要特性。


## ZooKeeper应用的典型场景

&emsp;&emsp;ZooKeeper 是一个高可用的分布式数据管理与协调框架。基于对ZAB算法的实现，该框架能够很好地保证分布式环境中数据的一致性。也是基于这样的特性，使得 ZooKeeper 成为了解决分布式一致性问题的利器。

### 数据发布与订阅（配置中心）

&emsp;&emsp;数据发布与订阅，即所谓的配置中心，顾名思义就是发布者将数据发布到 ZooKeeper 节点上,供订阅者进行数据订阅，进而达到动态获取数据的目的，实现配置信息的集中式管理和动态更新。
&emsp;&emsp;对于：数据量通常比较小。数据内容在运行时动态变化。集群中各机器共享，配置一致。这样的全局配置信息就可以发布到 ZooKeeper上，让客户端（集群的机器）去订阅该消息。

&emsp;&emsp;发布/订阅系统一般有两种设计模式，分别是推（Push）和拉（Pull）模式。
&emsp;&emsp;- 推模式
&emsp;&emsp;&emsp;&emsp;服务端主动将数据更新发送给所有订阅的客户端
&emsp;&emsp;- 拉模式
&emsp;&emsp;&emsp;&emsp;客户端主动发起请求来获取最新数据，通常客户端都采用定时轮询拉取的方式

&emsp;&emsp;ZooKeeper 采用的是推拉相结合的方式：
    &emsp;&emsp;客户端想服务端注册自己需要关注的节点，一旦该节点的数据发生变更，那么服务端就会向相应的客户端发送Watcher事件通知，客户端接收到这个消息通知后，需要主动到服务端获取最新的数据

### 命名服务

&emsp;&emsp;命名服务也是分布式系统中比较常见的一类场景。在分布式系统中，通过使用命名服务，客户端应用能够根据指定名字来获取资源或服务的地址，提供者等信息。被命名的实体通常可以是集群中的机器，提供的服务，远程对象等等——这些我们都可以统称他们为名字。

&emsp;&emsp;其中较为常见的就是一些分布式服务框架（如RPC）中的服务地址列表。通过在ZooKeepr里创建顺序节点，能够很容易创建一个全局唯一的路径，这个路径就可以作为一个名字。

&emsp;&emsp;ZooKeeper 的命名服务即生成全局唯一的ID。      

### 分布式协调服务/通知

&emsp;&emsp;ZooKeeper 中特有 Watcher 注册与异步通知机制，能够很好的实现分布式环境下不同机器，甚至不同系统之间的通知与协调，从而实现对数据变更的实时处理。使用方法通常是不同的客户端如果 机器节点 发生了变化，那么所有订阅的客户端都能够接收到相应的Watcher通知，并做出相应的处理。

&emsp;&emsp;ZooKeeper的分布式协调/通知，是一种通用的分布式系统机器间的通信方式。

### Master选举

&emsp;&emsp;Master 选举可以说是 ZooKeeper 最典型的应用场景了。比如 HDFS 中 Active NameNode 的选举、YARN 中 Active ResourceManager 的选举和 HBase 中 Active HMaster 的选举等。

&emsp;&emsp;针对 Master 选举的需求，通常情况下，我们可以选择常见的关系型数据库中的主键特性来实现：希望成为 Master 的机器都向数据库中插入一条相同主键ID的记录，数据库会帮我们进行主键冲突检查，也就是说，只有一台机器能插入成功——那么，我们就认为向数据库中成功插入数据的客户端机器成为Master。

&emsp;&emsp;依靠关系型数据库的主键特性确实能够很好地保证在集群中选举出唯一的一个Master。

&emsp;&emsp;但是，如果当前选举出的 Master 挂了，那么该如何处理？谁来告诉我 Master 挂了呢？显然，关系型数据库无法通知我们这个事件。但是，ZooKeeper 可以做到！

&emsp;&emsp;利用 ZooKeepr 的强一致性，能够很好地保证在分布式高并发情况下节点的创建一定能够保证全局唯一性，即 ZooKeeper 将会保证客户端无法创建一个已经存在的 数据单元节点。也就是说，如果同时有多个客户端请求创建同一个临时节点，那么最终一定只有一个客户端请求能够创建成功。利用这个特性，就能很容易地在分布式环境中进行 Master 选举了。

&emsp;&emsp;成功创建该节点的客户端所在的机器就成为了 Master。同时，其他没有成功创建该节点的客户端，都会在该节点上注册一个子节点变更的 Watcher，用于监控当前 Master 机器是否存活，一旦发现当前的Master挂了，那么其他客户端将会重新进行 Master 选举。这样就实现了 Master 的动态选举。

### 分布式锁

&emsp;&emsp;分布式锁是控制分布式系统之间同步访问共享资源的一种方式 分布式锁又分为排他锁和共享锁两种
&emsp;&emsp;排它锁 
&emsp;&emsp;ZooKeeper如何实现排它锁？

&emsp;&emsp;定义锁 
&emsp;&emsp;ZooKeeper 上的一个 机器节点 可以表示一个锁
&emsp;&emsp;获得锁 
&emsp;&emsp;把ZooKeeper上的一个节点看作是一个锁，获得锁就通过创建临时节点的方式来实现。 ZooKeeper 会保证在所有客户端中，最终只有一个客户端能够创建成功，那么就可以 认为该客户端获得了锁。同时，所有没有获取到锁的客户端就需要到/exclusive_lock 节点上注册一个子节点变更的Watcher监听，以便实时监听到lock节点的变更情况。
&emsp;&emsp;释放锁 
&emsp;&emsp;因为锁是一个临时节点，释放锁有两种方式

&emsp;&emsp;当前获得锁的客户端机器发生宕机或重启，那么该临时节点就会被删除，释放锁正常执行完业务逻辑后，客户端就会主动将自己创建的临时节点删除，释放锁。无论在什么情况下移除了lock节点，ZooKeeper 都会通知所有在 /exclusive_lock 节点上注册了节点变更 Watcher 监听的客户端。这些客户端在接收到通知后，再次重新发起分布式锁获取，即重复『获取锁』过程。

&emsp;&emsp;共享锁

&emsp;&emsp;共享锁在同一个进程中很容易实现，但是在跨进程或者在不同 Server 之间就不好实现了。Zookeeper 却很容易实现这个功能，实现方式也是需要获得锁的 Server 创建一个 EPHEMERAL_SEQUENTIAL 目录节点，然后调用 getChildren方法获取当前的目录节点列表中最小的目录节点是不是就是自己创建的目录节点，如果正是自己创建的，那么它就获得了这个锁，如果不是那么它就调用 exists(String path, boolean watch) 方法并监控 Zookeeper 上目录节点列表的变化，一直到自己创建的节点是列表中最小编号的目录节点，从而获得锁，释放锁很简单，只要删除前面它自己所创建的目录节点就行了。

## 小结
&emsp;&emsp;ZooKeeper 是一个开源的分布式协调服务，它实现诸如数据发布/订阅、负载均衡、命名服务、分布式协调/通知、集群管理、Master 选举、
配置维护，名字服务、分布式同步、分布式锁和分布式队列等功能。

&emsp;&emsp;zookeeper包含了三个角色，分别是Leader、Follower、以及Observer,默认只有Leader、Follower，在配置的时候，zoo.cfg文件中，他们的
区别就是myid中的server.{数值}=ip:端口中的数组的不同，首先通过一个Leader选举过程选出Leader机器，然后剩下的是follower机器，
要想获得Observer节点的话，需要配置文件中加peerType=Observer配置，Observer机器与Follower机器的区别就在于Observer不参与选举过成
也不参与写操作中的“过半成功”操作，所以Observer可以在不影响写性能提升集群的性能

&emsp;&emsp;客户端与服务端在进行tcp长连接后，然后客户端的生命周期也就开始了，客户端同时也通过心跳机制和服务器保持有效回话，在sessiontimeout
时间内，客户端如果因为某些原因导致连接断开，也能通过连接到任意一台服务器，之前的创建也会生效，节点中不光光会保存数据，也保存了自身的
一些状态信息

&emsp;&emsp;zookeeper中结构就是一个属性结构Leader是根节点，其余节点则是Follower节点，节点也分为持久节点以及临时节点，持久节点只有在主动删除才
会消失，而临时节点是在一个生命周期完结的时候就会消失，因为他是与客户端回话绑定的

&emsp;&emsp;zookeeper当中也存在事务操作与事件监听器。ZooKeeper允许用户在指定节点上注册一些 Watcher，并且在一些特定事件触发的时候，ZooKeeper 服务端
会将事件通知到感兴趣的客户端上去。该机制是 ZooKeeper 实现分布式协调服务的重要特性。

&emsp;&emsp;zookeeper采用的是cp策略，即一致性以及分区容错性，我们把服务注册到zookeeper中的节点中去，，这样客户端就能读取到其中的信息，只要机器节点
发生变化，那么订阅的客户端就能接收到相应的wacther同志，并作出处理