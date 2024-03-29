# 消费者和消费者群组

<mark style="color:blue;">**Kafka 消费者从属于消费者群组。一个群组里的消费者订阅的是同一个主题，每个消费者负责读取这个主题的部分消息。**</mark>

> ## <mark style="color:blue;">**向群组里添加消费者是横向扩展数据处理能力的主要方式**</mark>
>
> Kafka 消费者经常需要执行一些高延迟的操作，比如把数据写到数据库或用数据做一些比较耗时的计算。在这些情况下，单个消费者无法跟上数据生成的速度，因此可以增加更多的消费者来分担负载，让每个消费者只处理部分分区的消息，这是横向扩展消费者的主要方式。
>
> **可以为主题创建大量的分区，当负载急剧增长时，可以加入更多的消费者**。不过需要注意的是，<mark style="color:red;">**不要让消费者的数量超过主题分区的数量，因为多余的消费者只会被闲置**</mark>。

**不同于传统的消息系统，横向伸缩消费者和消费者群组并不会导致Kafka性能下降。**
