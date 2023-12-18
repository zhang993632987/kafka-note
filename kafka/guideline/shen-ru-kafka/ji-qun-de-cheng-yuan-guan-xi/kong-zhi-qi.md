# 控制器

* <mark style="color:blue;">**控制器其实也是一个broker，只不过除了提供一般的broker功能之外，它还负责选举分区首领**</mark>。
  * 集群中第一个启动的broker会通过在ZooKeeper中创建一个名为 <mark style="color:blue;">**/controller**</mark> 的临时节点让自己成为控制器。其他broker会在控制器节点上创建ZooKeeper watch。
  * 如果控制器被关闭或者与ZooKeeper断开连接，<mark style="color:blue;">**/controller**</mark> 临时节点就会消失。当临时节点消失时，集群中的其他broker将收到控制器节点已消失的通知，并尝试让自己成为新的控制器。
*   **每个新选出的控制器都会通过ZooKeeper条件递增操作获得一个数值更大的epoch**。

    * 其他broker也会知道当前控制器的epoch，如果收到由控制器发出的包含较小epoch的消息，就会忽略它们。这一点很重要，因为控制器会因长时间垃圾回收停顿与ZooKeeper断开连接——在停顿期间，新控制器将被选举出来。
    * 当旧控制器在停顿之后恢复时，它并不知道已经选出了新的控制器，并会继续发送消息——在这种情况下，旧控制器会被认为是一个“僵尸控制器”。

    **消息里的epoch可以用来忽略来自旧控制器的消息，这是防御“僵尸”的一种方式**。
* **控制器必须先从ZooKeeper加载最新的副本集状态，然后才能开始管理集群元数据和执行首领选举**。
  * 这个加载过程使用了异步API，为了减少延迟，请求会以管道的形式发送给ZooKeeper。
  * 但即便如此，**在有大量分区的集群中，加载过程可能仍然需要几秒**。
* **当控制器发现有一个broker离开了集群时，原先首领位于这个broker上的所有分区需要一个新首领。**
  * **它将遍历所有需要新首领的分区，并决定应该将哪个副本作为新首领**。
  * 然后，它会将更新后的状态持久化到ZooKeeper中，再向所有包含这些分区副本的broker发送一个LeaderAndISR请求，请求中包含了新首领和跟随者的信息。

总的来说，<mark style="color:blue;">**Kafka会使用ZooKeeper的临时节点来选举控制器，并会在broker加入或退出集群时通知控制器。控制器负责在broker加入或退出集群时进行首领选举。控制器会使用epoch来避免“脑裂”。**</mark>所谓的“脑裂”，就是指两个broker同时认为自己是集群当前的控制器。