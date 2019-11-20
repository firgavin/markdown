### grpc连接池



#### 精妙设计

1、状态变更直接返回一个channel，上层可以利用这个channel来资助进行判断；另外善用close channel也可以作为一种通知方式，closed channel要及时置为nil

2、大小锁

减小锁的范围，创建一个空的连接后立刻释放锁，然后再去实现这个连接。这对于网络来说很适用。需要注意race condition

3、多参数options写法（函数式选项模式）

4、连接池作用

- 快速get到连接，可以复用连接，可以维护状态
- grpc基于http2，协议上区分多个rpc

