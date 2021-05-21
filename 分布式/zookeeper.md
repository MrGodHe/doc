| 官方文档 | http://zookeeper.apache.org/doc/current/zookeeperOver.html |
| -------- | ---------------------------------------------------------- |

# zookeeper 是什么？
**官方定义：**

     ZooKeeper是一种用于分布式应用程序的分布式开源协调服务
     分布式应用程序可以基于 ZooKeeper 实现诸如数据发布/订阅、
     负载均衡、命名服务、分布式协调/通知、集群管理、Master 选举、分布式锁和分布式队列等功能。

# zookeeper 数据模型和分层命名空间
ZooKeeper提供的名称空间非常类似于标准文件系统。名称是由斜杠（/）分隔的路径元素序列。  
ZooKeeper名称空间中的每个节点都由路径标识
![](https://github.com/MrGodHe/doc/blob/master/image/zookeeper/zk_namespace.jpg)

## 节点模型- znode
与标准文件系统不同，ZooKeeper命名空间中的每个节点都可以包含与之关联的数据以及子项。
（存储在每个节点的数据通常很小，在字节到千字节范围内）

Znodes维护一个stat结构（统计结构）

    cZxid = 0xb63                           //创建znode的更改的事务ID
    ctime = Fri Sep 20 16:58:09 CST 2019    //创建znode的时间 (以毫秒为单位)
    mZxid = 0xb67                           //znode最后一次修改的更改的事务ID
    mtime = Fri Sep 20 17:03:32 CST 2019    //znode最后一次修改时间 (以毫秒为单位)
    pZxid = 0xb64                           //znode关于添加和移除子结点更改的事务ID
    cversion = 1                            //znode子结点变化的次数     
    dataVersion = 1                         //znode上数据变化的次数
    aclVersion = 0                          //znode结点上ACL变化的次数
    ephemeralOwner = 0x0                    //如果znode是短暂类型的结点，这代表了znode拥有者的session ID，
                                            //如果znode不是短暂类型的结点，这个值为0
    dataLength = 9                          //znode数据域的长度
    numChildren = 1                         //znode中子结点的个数

## 节点类型

1. **持久节点（PERSISTENT）**

   节点创建后便一直存在于Zookeeper服务器上，直到有删除操作来主动清楚该节点。

2. **持久顺序节点（PERSISTENT_SEQUENTIAL）**

   相比持久节点，其新增了顺序特性，每个父节点都会为它的第一级子节点维护一份顺序， 用于记录每个子节点创建的先后顺序。
   在创建节点时，会自动添加一个数字后缀，作为新的节点名，该数字后缀的上限是整形的最大值。

3. **临时节点（EPEMERAL）**

   临时节点的生命周期与客户端会话绑定，客户端失效，节点会被自动清理。
   同时，Zookeeper规定不能基于临时节点来创建子节点，即临时节点只能作为叶子节点。

4. **临时顺序节点（EPEMERAL_SEQUENTIAL）**

   在临时节点的基础添加了顺序特性。

## 访问控制列表（Access Control Lists,ACL）
ZK的节点有5种操作权限：
CREATE、READ、WRITE、DELETE、ADMIN 也就是 增、删、改、查、管理权限，这5种权限简写为crwda(即：每个单词的首字符缩写)
注：这5种权限中，delete是指对子节点的删除权限，其它4种权限指对自身节点的操作权限

ZK的每个节点都包括一个ACL列表，

    ① 权限Permission
    ② 验证权限模式scheme
    ③ 授权对象（ID）`
ACL由（scheme：expression，perms）对组成。表达式的格式特定于该方案。
例如，该对（ip：19.22.0.0/16，READ）为IP地址以19.22开头的任何客户端提供READ权限

**授权对象**
是指权限赋予的用户或一个指定实体，如IP地址或机器等。不同的权限模式通常有不同的授权对象。
权限模式用来确定权限验证过程中使用的检验策略，有如下四种模式：

    1. IP，通过IP地址粒度来进行权限控制，如"ip:192.168.0.110"表示权限控制针对该IP地址，
       同时IP模式可以支持按照网段方式进行配置，如"ip:192.168.0.1/24"表示针对192.168.0.*这个网段进行权限控制。
    2. Digest，使用"username:password"形式的权限标识来进行权限配置，便于区分不同应用来进行权限控制。
       Zookeeper会对其进行SHA-1加密和BASE64编码。
    3. World，最为开放的权限控制模式，数据节点的访问权限对所有用户开放。
    4. Super，超级用户，是一种特殊的Digest模式，超级用户可以对任意Zookeeper上的数据节点进行任何操作。

# zookeeper 连接会话（Session）
 Session 指的是 ZooKeeper 服务器与客户端会话。在 ZooKeeper 中一个客户端连接是指客户端和服务器之间的一个 TCP 长连接。

    客户端启动的时候，首先会与服务器建立一个 TCP 连接，从第一次连接建立开始，客户端会话的生命周期也开始了。
    通过这个连接，客户端能够通过心跳检测与服务器保持有效的会话，
    也能够向Zookeeper服务器发送请求并接受响应，同时还能够通过该连接接收来自服务器的Watch事件通知。
    
    Session的 sessionTimeout值用来设置一个客户端会话的超时时间。
    当由于服务器压力太大、网络故障或是客户端主动断开连接等各种原因导致客户端连接断开时，
    只要在sessionTimeout规定的时间内能够重新连接上集群中任意一台服务器，那么之前创建的会话仍然有效

# zookeeper 工作原理
## **一. 集群结构**  

简单分为两类：

1.  唯一一个是Leader（通过内部选举确定的）
2.  其余全是follower

     Leader和各个follower是互相通信的，对于Zookeeper系统的数据都是保存在内存里面的，同样也会备份一份在磁盘上
     如果Leader挂了，Zookeeper集群会重新选举，在毫秒级别就会重新选举出一个Leader
     集群中除非有一半以上的Zookeeper节点挂了，Zookeeper Service才不可用

## **二. 数据的读写**

-  **写数据**

  一个客户端进行写数据请求时，如果是follower接收到写请求，就会把请求转发给Leader，Leader通过内部的Zab协议进行原子广播，直到所有Zookeeper节点都成功写了数据后（内存同步以及磁盘更新），这次写请求算是完成，然后Zookeeper Service就会给Client发回响应。

- **读数据**

  由于写入据保持一致，读数据可以有任何一台zk节点提供 

## **三. 核心处理（广播）**

​    Zab（ZooKeeper Atomic Broadcast）原子消息广播协议作为数据一致性的核心算法，
​    Zab 协议是专为Zookeeper设计的支持崩溃恢复原子消息广播算法 。

    所有的事务请求必须一个全局唯一的服务器（Leader）来协调处理，集群其余的服务器称为follower服务器。
    Leader服务器负责将一个客户端请求转化为事务提议（Proposal），并将该proposal分发给集群所有的follower服务器。之后Leader服务器需要等待所有的follower服务器的反馈，一旦超过了半数的follower服务器进行了正确反馈后，那么Leader服务器就会再次向所有的follower服务器分发commit消息，要求其将前一个proposal进行提交。

![](https://github.com/MrGodHe/doc/blob/master/image/zookeeper/zk_Atomic_Broadcast.jpg)

## **四. 领导选举（leader）**

1. 什么条件先会出现领导选举？
   - zk服务初始化启动
   - zk服务运行期间无法和leader保持连接
2. 出现领导选举时，集群中的状态？
   - zk集群中已有leader
   - zk集群中无leader

    有leader情况： 比较简单，服务器会被告知集群中已经有leader，你只需要重新和leader建立连接，并同步状态即可。
    无leader情况： 比较复杂....
                  (1) 第一次投票：所有服务器变为LOOKING状态，该状态的服务器都会想其他所有服务器发送消息【称为投票】。
                                   投票的内容包括SID（服务器唯一标识）和 ZXID（事务ID）
                  (2) 变更投票：每台服务器发出投票以外，同时会收到其他机器的投票， 并根据一系列规则来决定是否变更自己的投票。
                  术语：
                        ·vote_sid： 接收到的投票中所推举Leader服务器的SID。
                        ·vote_zxid：接收到的投票中所推举Leader服务器的ZXID、
                        ·self_sid： 当前服务器自己的SID
                        ·self_zxid：当前服务器自己的ZXID
                  规则：
                        ·规则一：如果vote_zxid大于self_zxid，就认可当前收到的投票，并再次将该投票发送出去 
                        ·规则二：如果vote_zxid小于self_zxid，那么坚持自己的投票，不做任何变更
                        ·规则三：如果vote_zxid等于self_zxid，那么就对比两者的SID，如果vote_sid大于self_sid，
                                  那么就认可当前收到的投票，并再次将该投票发送出去。   
                        ·规则四：如果vote_zxid等于self_zxid，并且vote_sid小于self_sid，
                                  那么坚持自己的投票，不做任何变更。               
                  (3)确定领导：
                        经过第二轮投票后，集群中的每台机器都会再次接收到其他机器的投票，然后开始统计投票，
                        如果一台机器收到了超过半数的相同投票，那么这个投票对应的SID机器即为Leader

# zookeeper 数据的发布和订阅                       

# zookeeper 分布式（写锁、CAP -> CP）

 **CAP原则又称CAP定理**

```
指的是在一个分布式系统中，一致性（Consistency）、可用性（Availability）、分区容错性（Partition tolerance）。
CAP 原则指的是，这三个要素最多只能同时实现两点，不可能三者兼顾。
```

   **Lock :**

![](https://github.com/MrGodHe/doc/blob/master/image/zookeeper/zk_writeLock.jpg)

        (1).基于一个根节点“/locknode”上创建 临时且有序“EPEMERAL_SEQUENTIAL”节点
        (2).获取locknode根节点的 getChildren()子节点列表，并不设置Watcher（避免羊群效应）
        (3).如果当前客户端节点是子节点列表中最小序号的，则得到锁资源
        (4).如果当前客户端节点不是最小序号，则判断前一个节点exist(),并设置Watcher
        (5).如果前一个节点exist是 null, 则转到2步骤;

   **unLock:**

        delete();删除当前客户端节点

