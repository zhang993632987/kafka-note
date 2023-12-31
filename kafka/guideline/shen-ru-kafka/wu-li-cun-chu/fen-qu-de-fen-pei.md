# 分区的分配

在创建主题时，Kafka首先要决定如何在broker间分配分区。

**假设你有6个broker，打算创建一个包含10个分区的主题，并且复制系数为3，那么总共会有30个分区副本，它们将被分配给6个broker**，此时，分配方案如下：

* <mark style="color:blue;">**先随机选择一个broker（假设是4），然后使用轮询的方式给每个broker分配分区首领**</mark>。于是：
  * 分区0的首领在broker 4上
  * 分区1的首领在broker 5上
  * 分区2的首领在broker 0上（因为只有6个broker）
  * 以此类推
* <mark style="color:blue;">**接下来，从分区首领开始，依次分配跟随者副本**</mark>。
  * 如果分区0的首领在broker 4上，那么它的第一个跟随者副本就在broker 5上，第二个跟随者副本就在broker 0上。
  * 如果分区1的首领在broker 5上，那么它的第一个跟随者副本就在broker 0上，第二个跟随者副本在broker 1上。

{% hint style="info" %}
## <mark style="color:blue;">提示</mark>

<mark style="color:blue;">**如果配置了机架信息，那么就不是按照数字顺序而是按照机架交替的方式来选择broker了**</mark>。假设broker 0和broker 1被放置在一个机架上，broker 2和broker 3被放置在另一个机架上。我们不是按照从0到3的顺序来选择broker，而是按照0、2、1、3的顺序来选择，以保证相邻的broker总是位于不同的机架上。

于是，如果分区0的首领在broker 2上，那么第一个跟随者副本就在broker 1上，以保证它们位于不同的机架上。因为如果第一个机架离线，则还有其他幸存的副本，所以分区仍然可用。这对所有副本来说都是一样的，因此在机架离线时仍然能够保证可用性。
{% endhint %}

为分区和副本选好合适的broker之后，接下来要决定新分区应该被放在哪个目录。我们会为每个分区分配目录，规则很简单：**计算每个目录里的分区数量，新分区总是会被放在分区数量最少的那个目录**。也就是说，如果添加了一个新磁盘，那么所有新分区都会被放到这个磁盘。这是因为在达到均衡分配的状态之前，新磁盘的分区数量总是最少的。
