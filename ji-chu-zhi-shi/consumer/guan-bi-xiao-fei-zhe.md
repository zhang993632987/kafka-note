# 关闭消费者

<mark style="color:orange;">**consumer.wakeup() 是消费者唯一一个可以在其他线程中安全调用的方法：**</mark>

* **如果你确定马上要关闭消费者（即使消费者还在等待一个 poll() 返回），那么可以在另一个线程中调用 consumer.wakeup()**。
* **如果轮询循环运行在主线程中，那么可以在 ShutdownHook 里调用这个方法。**

{% hint style="info" %}
## <mark style="color:orange;">注意</mark>

<mark style="color:orange;">**调用 consumer.wakeup() 会导致 poll() 抛出 WakeupException，如果调用consumer.wakeup() 时线程没有在轮询，那么异常将在下一次调用 poll() 时抛出。**</mark>

不一定要处理 WakeupException，但在<mark style="color:orange;">**退出线程之前必须调用 consumer.close()**</mark>。

* **消费者在被关闭时会提交还没有提交的偏移量，并向消费者协调器发送消息，告知自己正在离开群组。**
* **协调器会立即触发再均衡，被关闭的消费者所拥有的分区将被重新分配给群组里其他的消费者，不需要等待会话超时。**
{% endhint %}

<details>

<summary><mark style="color:purple;">示例</mark></summary>

```java
Runtime.getRuntime().addShutdownHook(new Thread() {
   public void run() {
       System.out.println("Starting exit...");
       consumer.wakeup(); 
       try {
           mainThread.join();
       } catch (InterruptedException e) {
           e.printStackTrace();
       }
   }
});
​
...
Duration timeout = Duration.ofMillis(10000); 
​
try {
   // 一直循环，直到按下Ctrl-C组合键，关闭钩子会在退出时做清理工作
   while (true) {
       ConsumerRecords<String, String> records =
           movingAvg.consumer.poll(timeout);
       System.out.println(System.currentTimeMillis() +
           "-- waiting for data...");
       for (ConsumerRecord<String, String> record : records) {
           System.out.printf("offset = %d, key = %s, value = %s\n",
               record.offset(), record.key(), record.value());
       }
       for (TopicPartition tp: consumer.assignment())
           System.out.println("Committing offset at position:" +
               consumer.position(tp));
       movingAvg.consumer.commitSync();
   }
} catch (WakeupException e) {
   // 忽略异常 
} finally {
   // 在退出之前，确保彻底关闭了消费者
   consumer.close(); 
   System.out.println("Closed consumer and we are done");
}
```

</details>
