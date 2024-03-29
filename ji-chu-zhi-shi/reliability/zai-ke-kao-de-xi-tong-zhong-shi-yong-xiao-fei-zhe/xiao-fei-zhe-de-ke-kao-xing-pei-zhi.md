# 消费者的可靠性配置

为了保证消费者行为的可靠性，需要注意以下 4 个非常重要的配置参数：

* <mark style="color:blue;">**group.id**</mark>：如果两个消费者具有相同的群组 ID，并订阅了同一个主题，那么每个消费者将分到主题分区的一个子集，也就是说它们只能读取到所有消息的一个子集（但整个群组可以读取到主题所有的消息）。**如果你希望一个消费者可以读取主题所有的消息，那么就需要为它设置唯一的 group.id。**
* <mark style="color:blue;">**auto.offset.reset**</mark>：这个参数指定了当没有偏移量（比如在消费者首次启动时）或请求的偏移量在broker 上不存在时消费者该作何处理。这个参数有两个值：
  * 一个是 **earliest**，如果配置了这个值，那么消费者将从分区的开始位置读取数据，这会导致消费者读取大量的重复数据，但可以保证最少的数据丢失。
  * 另一个值是 **latest**，如果配置了这个值，那么消费者将从分区的末尾位置读取数据，这样可以减少重复处理消息，但很有可能会错过一些消息。
* <mark style="color:blue;">**enable.auto.commit**</mark>：可以决定让消费者自动提交偏移量，也可以在代码里手动提交偏移量。
  * 自动提交的一个最大好处是可以少操心一些事情。**如果是在消费者的消息轮询里处理数据，那么自动提交可以确保不会意外提交未处理的偏移量。自动提交的主要缺点是我们无法控制应用程序可能重复处理的消息的数量，比如消费者在还没有触发自动提交之前处理了一些消息，然后被关闭。**
  * 如果应用程序的处理逻辑比较复杂（比如把消息交给另外一个后台线程去处理），那么就只能使用手动提交了，因为自动提交机制有可能会在还没有处理完消息时就提交偏移量。
* <mark style="color:blue;">**auto.commit.interval.ms**</mark>：如果选择使用自动提交，那么可以通过这个参数来控制提交的频率，默认每 5 秒提交一次。一般来说，**频繁提交会增加额外的开销，但也会降低重复处理消息的概率**。
