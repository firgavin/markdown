- 一致性协议
  - 保证集群中**大部分节点可用**的情况下任然可以正常work
  - 半数以上节点 = 大部分节点可用
  - Paxos（Chubby） & Raft（etcd）



### Leader节点选举

- 节点角色
  - leader
  - follower
  - candidate

- 工作模式
  - 一个leader，多个follower
  - 每次leader的产生伴随term（任期）的自增（选举失败时term也会更新）

- 两个超时时间
  - election timeout，选举超时时间，节点自主维护，超时则认为该term的结束
  - hearbeat timeout，心跳超时时间，leader向follower发送心跳的interval



Q：leader是怎么产生的

A：通过选举产生。当集群节点长时间没有接受到leader的心跳时，election timeout从follower节点转变为candidate状态



Q：client怎么与集群进行交互

A：Raft中提及两种模式：重定向到leader、proxy模式。etcd实现方式为前者



Q：事务如何提交，如何保证集群中各个节点的一致性并兼顾效率

A：log、超过半数、以及事务状态（uncommitted -> committed）



Q：leader宕机时集群如何恢复正常、原宕机leader后来恢复如何同步log

A：重新选leader的过程



Q：leader节点频繁宕机等原因导致频繁选举的产生

A：Raft要求时间保证`hearbeat timeout << election timeout << 平均故障间隔时间`





### 日志复制



### 网络分区



### Eage Case及各种极端情况

