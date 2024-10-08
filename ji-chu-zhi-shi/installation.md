# 安装

## 目录说明

* /opt/module：存放下载的 jar 包
* /opt/software：应用放置于此
* /opt/etc/kafka：配置、数据以及日志存放路径（只是为了方便分发所以放到了一个目录下，推荐分开放置）
* /opt/etc/kafka/data：持久化的消息日志存放路径
* /opt/etc/kafka/logs：运行日志，log4j 日志

## 下载并解压

```bash
cd /opt/module
wget https://downloads.apache.org/kafka/3.5.1/kafka_2.12-3.5.1.tgz
```

```bash
tar -xzv -f kafka_2.12-3.5.1.tgz -C /opt/software/
```

```bash
ln -s /opt/software/kafka_2.12-3.5.1 /opt/software/kafka
```

## 配置环境变量

```bash
sudo vim /etc/profile.d/kafka.sh
```

```bash
KAFKA_HOME=/opt/software/kafka
export PATH=$PATH:$KAFKA_HOME/bin
```

## 配置文件

```bash
mkdir /opt/etc/kafka

# 将config文件夹变成一个指向/opt/etc/kafka/config的软连接
mv /opt/software/kafka/config /opt/etc/kafka/
ln -s /opt/etc/kafka/config /opt/software/kafka/config
```

> <pre class="language-bash"><code class="lang-bash"><strong>mv /opt/software/kafka/config /opt/etc/kafka/
> </strong></code></pre>
>
> 之所以采用外置配置文件，是为了方便 kafka 服务的升级与替换。

修改 <mark style="color:blue;">**server.properies**</mark> 中的配置内容：

```properties
# 集群中个各个 broker 必须拥有不同的 id
broker.id=3
  ​
listeners=PLAINTEXT://0.0.0.0:9092
advertised.listeners=PLAINTEXT://hadoop103:9092
  ​
# 存放
log.dirs=/opt/etc/kafka/data
  ​
zookeeper.connect=hadoop101:2181,hadoop102:2181,hadoop103:2181/kafka
```

{% hint style="info" %}
**要将一个 broker 加入到集群里，只需要修改以下三个配置参数：**

* [x] <mark style="color:blue;">**zookeeper.connect**</mark>：所有 broker 都必须配置相同的值
* [x] <mark style="color:blue;">**broker.id**</mark>：每一个 broker 都必须有一个唯一的 ID
* [x] <mark style="color:blue;">**advertised.listeners**</mark>**：**broker 监听的 IP 地址和端口
{% endhint %}

## 集群脚本kf.sh

```bash
vim ~/bin/kf.sh
```

```bash
#!/bin/bash

KAFKA_HOME=/opt/software/kafka
export PATH=$PATH:$KAFKA_HOME/bin

KAFKA_CONFIG=$KAFKA_HOME/config/server.properties

case $1 in
"start"){
    for i in hadoop101 hadoop102 hadoop103
    do
        echo " --------启动 $i Kafka-------"
        ssh $i "kafka-server-start.sh -daemon $KAFKA_CONFIG"
    done
};;
"stop"){
    for i in hadoop101 hadoop102 hadoop103
    do
        echo " --------停止 $i Kafka-------"
        ssh $i "kafka-server-stop.sh stop"
    done
};;
esac
```
