# 持久化到磁盘

即使消息还没有被持久化到磁盘上，Kafka 也可以向生产者发出确认，这取决于已接收到消息的副本的数量。

<mark style="color:blue;">**Kafka 会在重启之前和关闭日志片段（默认 1 GB 大小时关闭）时将消息冲刷到磁盘上，或者等到 Linux 系统页面缓存被填满时冲刷**</mark>**。**其背后的想法是，拥有 3 台放置在不同机架或可用区域的机器，并在每台机器上放置一份数据副本比只将消息写入首领的磁盘更加安全，因为两个不同的机架或可用区域同时发生故障的可能性非常小。

不过，也可以让 broker 更频繁地将消息持久化到磁盘上：

* 配置参数 <mark style="color:blue;">**flush.messages**</mark> 用于控制未同步到磁盘的最大消息数量
* <mark style="color:blue;">**flush.ms**</mark> 用于控制同步频率

在配置这些参数之前，最好先了解一下 <mark style="color:blue;">**fsync**</mark> 是如何影响 Kafka 的吞吐量的以及如何尽量避开它的缺点。
