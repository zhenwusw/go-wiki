1 etcd 概念词汇表

* Raft
* Node
* Memeber
* Cluster
* Peer
* Client
* WAL
* snapshot
* Proxy
* Leader
* Follower
* Candidate
* Term
* Index: 数据项编号。Raft 中通过 Term 和 Index 来定位数据



2 集群化应用实践

2.1 集群启动

* 部署时推荐部署奇数个节点的集群：3，5，7
* 摒弃了使用配置文件进行参数配置的做法，转而使用命令行参数或者环境变量的做法来配置参数。

2.1.1 静态配置

适用于离线环境，在启动整个集群之前，你就已经预先清楚所要配置的集群大小，以及集群上各节点的地址和端口信息。

```bash
ETCD_INITIAL_CLUSTER="infra0=http://10.0.1.10:2380,infra1=http://10.0.1.11:2380,infra2=http://10.0.1.12:2380"
ETCD_INITIAL_CLUSTER_STATE=new
```



3. 关键部分源码分析

 载入配置参数: `etcdmain/etcd.go`
 
 Proxy mode: startProxy函数，否则进入startEtcd
 
 启动http监听, 保持与集群其他etcd机器（peers）保持连接

 为何要使用 `transport.NewTimeoutListener` 和 `transport.NewKeepAliveListener` 启动和监听?

 在etcdmain/etcd.go中的setupCluster函数可以看到，根据不同etcd的参数，启动集群的方法略有不同



Leader节点在提交一个写记录时，会把这个消息同步到每个节点上，当得到多数节点的同意反馈后，才会真正写入数据。所以节点越多，写入性能越差。

当集群超过半数的节点都失效时，就需要通过手动的方式，强制性让某个节点以自己为Leader，利用原有数据启动一个新集群。


5 Proxy 模式








## 参考文献

1. http://www.infoq.com/cn/articles/etcd-interpretation-application-scenario-implement-principle
