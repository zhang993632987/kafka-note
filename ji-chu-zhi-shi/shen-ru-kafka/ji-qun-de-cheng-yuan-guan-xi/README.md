# 集群的成员关系

<mark style="color:blue;">**Kafka 使用 ZooKeeper 维护集群的成员信息：**</mark>

* 每个 broker 都有一个唯一的标识符，这个标识符既可以在配置文件中指定，也可以自动生成。**broker在启动时通过创建 ZooKeeper 临时节点把自己的 ID 注册到 ZooKeeper 中**。
* broker、控制器和其他的一些生态系统工具会订阅 ZooKeeper 的 <mark style="color:blue;">**/brokers/ids**</mark> 路径（也就是 broker 在ZooKeeper上的注册路径），当有 broker 加入或退出集群时，它们可以收到通知。
* 当 broker 与 ZooKeeper 断开连接时，它在启动时创建的临时节点会自动从 ZooKeeper 上移除。监听 broker 节点路径的 Kafka 组件会被告知这个 broker 已被移除。

> broker 对应的 ZooKeeper 节点会在 broker 被关闭之后消失，但它的 ID 会**继续存在于其他数据结构**中。例如，每个主题的副本集中就可能包含这个 ID。
>
> **在完全关闭一个 broker 后，如果使用相同的 ID 启动另一个全新的 broker，则它会立即加入集群，并获得与之前相同的分区和主题。**
