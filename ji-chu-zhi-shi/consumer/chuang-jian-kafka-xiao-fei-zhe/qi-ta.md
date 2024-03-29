# 其他

## <mark style="color:blue;">**group.id**</mark>

group.id 不是必需的，它指定了**一个消费者属于哪一个消费者群组**。

可以创建不属于任何一个群组的消费者，只是这种做法不太常见。

## <mark style="color:blue;">**group.instance.id**</mark>

这个属性可以是任意具有唯一性的字符串，它**为消费者指定了一个固定的唯一值**。

{% content-ref url="../xiang-guan-gai-nian/qun-zu-gu-ding-cheng-yuan.md" %}
[qun-zu-gu-ding-cheng-yuan.md](../xiang-guan-gai-nian/qun-zu-gu-ding-cheng-yuan.md)
{% endcontent-ref %}

## <mark style="color:blue;">**partition.assignment.strategy**</mark>

PartitionAssignor 根据给定的消费者和它们订阅的主题来决定哪些分区应该被分配给哪个消费者。

Kafka提供了几种默认的分配策略。

### <mark style="color:blue;">区间</mark><mark style="color:blue;">**(range)**</mark>

这个策略会**把每一个主题的若干个连续分区分配给消费者**。

假设消费者 C1 和消费者 C2 同时订阅了主题 T1 和主题 T2，并且每个主题有 3 个分区，那么：

* 消费者 C1 有可能会被分配到这两个主题的分区 0 和分区 1
* 消费者 C2 则会被分配到这两个主题的分区 2

因为每个主题拥有奇数个分区，并且都遵循一样的分配策略，所以第一个消费者会分配到比第二个消费者更多的分区。**只要使用了这个策略，并且分区数量无法被消费者数量整除，就会出现这种情况。**

### <mark style="color:blue;">**轮询(roundRobin)**</mark>

这个策略会**把所有被订阅的主题的所有分区按顺序逐个分配给消费者**。

如果使用轮询策略为消费者 C1 和消费者 C2 分配分区，那么

* 消费者 C1 将分配到主题 T1 的分区 0 和分区 2 以及主题 T2 的分区 1
* 消费者 C2 将分配到主题 T1 的分区 1 以及主题 T2 的分区 0 和分区 2

**一般来说，如果所有消费者都订阅了相同的主题（这种情况很常见），那么轮询策略会给所有消费者都分配相同数量（或最多就差一个）的分区。**

### <mark style="color:blue;">**黏性(sticky)**</mark>

设计黏性分区分配器的目的有两个：

* 一是尽可能均衡地分配分区；
* 二是<mark style="color:blue;">**在进行再均衡时尽可能多地保留原先的分区所有权关系，减少将分区从一个消费者转移给另一个消费者所带来的开销**</mark>。

**如果所有消费者都订阅了相同的主题，那么黏性分配器初始的分配比例将与轮询分配器一样均衡。后续的重新分配将同样保持均衡，但减少了需要移动的分区的数量。**

**如果同一个群组里的消费者订阅了不同的主题，那么黏性分配器的分配比例将比轮询分配器更加均衡。**

### <mark style="color:blue;">**协作黏性(cooperative sticky)**</mark>

**这个分配策略与黏性分配器一样，只是它**<mark style="color:orange;">**支持协作（增量式）再均衡**</mark>，**在进行再均衡时消费者可以继续从没有被重新分配的分区读取消息。**

## <mark style="color:blue;">**client.id**</mark>

这个属性可以是任意字符串，**broker 用它来标识从客户端发送过来的请求**，比如获取请求。

它通常被用在日志、指标和配额中。

## <mark style="color:blue;">**client.rack**</mark>

> **在默认情况下，消费者会从每个分区的首领副本那里获取消息**。但是，如果集群跨越了多个数据中心或多个云区域，那么**让消费者从位于同一区域的副本那里获取消息就会具有性能和成本方面的优势。**

**要从最近的副本获取消息，需要：**

* **设置 client.rack 这个参数，用于标识客户端所在的区域**
* **将 broker 的 replica.selector.class 参数值改为org.apache.kafka.common.replica.RackAwareReplicaSelector**

## <mark style="color:blue;">**default.api.timeout.ms**</mark>

**如果在调用消费者 API 时没有显式地指定超时时间，那么消费者就会在调用其他 API 时使用这个属性指定的值。默认值是 1 分钟**，因为它比请求超时时间的默认值大，所以可以将重试时间包含在内。

> **poll() 方法是一个例外，因为它需要显式地指定超时时间。**

## <mark style="color:blue;">**request.timeout.ms**</mark>

这个属性指定了**消费者在收到 broker 响应之前可以等待的最长时间**。如果 broker 在指定时间内没有做出响应，那么客户端就会关闭连接并尝试重连。它的**默认值是 30 秒**。

> 不建议把它设置得比默认值小。在放弃请求之前要给 broker 留有足够长的时间来处理其他请求，因为向已经过载的 broker 发送请求几乎没有什么好处，况且断开并重连只会造成更大的开销。

## <mark style="color:blue;">**receive.buffer.bytes**</mark>** 和 **<mark style="color:blue;">**send.buffer.bytes**</mark>

这两个属性分别指定了 **socket 在读写数据时用到的 TCP 缓冲区大小**。

* 如果它们被设置为 –1，就使用操作系统的默认值。
* 如果生产者或消费者与 broker 位于不同的数据中心，则可以适当加大它们的值，因为跨数据中心网络的延迟一般都比较高，而带宽又比较低。
