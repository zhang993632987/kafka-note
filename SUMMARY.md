# Table of contents

## 基础知识

* [概述](README.md)
  * [相关概念](ji-chu-zhi-shi/gai-shu/xiang-guan-gai-nian.md)
  * [Kafka 的特性](ji-chu-zhi-shi/gai-shu/kafka-de-te-xing.md)
* [安装](ji-chu-zhi-shi/installation.md)
* [生产者（Producer）](ji-chu-zhi-shi/produce/README.md)
  * [创建 Kafka 生产者](ji-chu-zhi-shi/produce/chuang-jian-kafka-sheng-chan-zhe/README.md)
    * [三个必须设置的属性](ji-chu-zhi-shi/produce/chuang-jian-kafka-sheng-chan-zhe/san-ge-bi-xu-she-zhi-de-shu-xing.md)
    * [消息传递时间](ji-chu-zhi-shi/produce/chuang-jian-kafka-sheng-chan-zhe/xiao-xi-chuan-di-shi-jian.md)
    * [其他属性](ji-chu-zhi-shi/produce/chuang-jian-kafka-sheng-chan-zhe/qi-ta-shu-xing.md)
  * [消息的发送方式](ji-chu-zhi-shi/produce/xiao-xi-de-fa-song-fang-shi.md)
  * [分区](ji-chu-zhi-shi/produce/fen-qu.md)
  * [标头](ji-chu-zhi-shi/produce/biao-tou.md)
  * [拦截器](ji-chu-zhi-shi/produce/lan-jie-qi.md)
  * [配额和节流](ji-chu-zhi-shi/produce/peiehe-jie-liu.md)
* [消费者（Consumer）](ji-chu-zhi-shi/consumer/README.md)
  * [相关概念](ji-chu-zhi-shi/consumer/xiang-guan-gai-nian/README.md)
    * [消费者和消费者群组](ji-chu-zhi-shi/consumer/xiang-guan-gai-nian/xiao-fei-zhe-he-xiao-fei-zhe-qun-zu.md)
    * [消费者群组和分区再均衡](ji-chu-zhi-shi/consumer/xiang-guan-gai-nian/xiao-fei-zhe-qun-zu-he-fen-qu-zai-jun-heng.md)
    * [群组固定成员](ji-chu-zhi-shi/consumer/xiang-guan-gai-nian/qun-zu-gu-ding-cheng-yuan.md)
  * [创建Kafka消费者](ji-chu-zhi-shi/consumer/chuang-jian-kafka-xiao-fei-zhe/README.md)
    * [三个必须设置的属性](ji-chu-zhi-shi/consumer/chuang-jian-kafka-xiao-fei-zhe/san-ge-bi-xu-she-zhi-de-shu-xing.md)
    * [会话的相关属性](ji-chu-zhi-shi/consumer/chuang-jian-kafka-xiao-fei-zhe/hui-hua-de-xiang-guan-shu-xing.md)
    * [影响吞吐量的属性](ji-chu-zhi-shi/consumer/chuang-jian-kafka-xiao-fei-zhe/ying-xiang-tun-tu-liang-de-shu-xing.md)
    * [偏移量的相关属性](ji-chu-zhi-shi/consumer/chuang-jian-kafka-xiao-fei-zhe/pian-yi-liang-de-xiang-guan-shu-xing.md)
    * [其他](ji-chu-zhi-shi/consumer/chuang-jian-kafka-xiao-fei-zhe/qi-ta.md)
  * [订阅主题](ji-chu-zhi-shi/consumer/ding-yue-zhu-ti.md)
  * [轮询](ji-chu-zhi-shi/consumer/lun-xun/README.md)
    * [提交和偏移量](ji-chu-zhi-shi/consumer/lun-xun/ti-jiao-he-pian-yi-liang.md)
    * [再均衡监听器](ji-chu-zhi-shi/consumer/lun-xun/zai-jun-heng-jian-ting-qi.md)
    * [从特定偏移量位置读取记录](ji-chu-zhi-shi/consumer/lun-xun/cong-te-ding-pian-yi-liang-wei-zhi-du-qu-ji-lu.md)
  * [关闭消费者](ji-chu-zhi-shi/consumer/guan-bi-xiao-fei-zhe.md)
* [深入Kafka](ji-chu-zhi-shi/shen-ru-kafka/README.md)
  * [集群的成员关系](ji-chu-zhi-shi/shen-ru-kafka/ji-qun-de-cheng-yuan-guan-xi/README.md)
    * [控制器](ji-chu-zhi-shi/shen-ru-kafka/ji-qun-de-cheng-yuan-guan-xi/kong-zhi-qi.md)
    * [复制](ji-chu-zhi-shi/shen-ru-kafka/ji-qun-de-cheng-yuan-guan-xi/fu-zhi.md)
  * [处理请求](ji-chu-zhi-shi/shen-ru-kafka/chu-li-qing-qiu/README.md)
    * [生产请求](ji-chu-zhi-shi/shen-ru-kafka/chu-li-qing-qiu/sheng-chan-qing-qiu.md)
    * [获取请求](ji-chu-zhi-shi/shen-ru-kafka/chu-li-qing-qiu/huo-qu-qing-qiu.md)
  * [物理存储](ji-chu-zhi-shi/shen-ru-kafka/wu-li-cun-chu/README.md)
    * [分区的分配](ji-chu-zhi-shi/shen-ru-kafka/wu-li-cun-chu/fen-qu-de-fen-pei.md)
    * [文件管理](ji-chu-zhi-shi/shen-ru-kafka/wu-li-cun-chu/wen-jian-guan-li.md)
    * [文件格式](ji-chu-zhi-shi/shen-ru-kafka/wu-li-cun-chu/wen-jian-ge-shi.md)
    * [索引](ji-chu-zhi-shi/shen-ru-kafka/wu-li-cun-chu/suo-yin.md)
    * [分层存储](ji-chu-zhi-shi/shen-ru-kafka/wu-li-cun-chu/fen-ceng-cun-chu.md)
  * [压实](ji-chu-zhi-shi/shen-ru-kafka/ya-shi/README.md)
    * [工作原理](ji-chu-zhi-shi/shen-ru-kafka/ya-shi/gong-zuo-yuan-li.md)
    * [删除键](ji-chu-zhi-shi/shen-ru-kafka/ya-shi/shan-chu-jian.md)
    * [何时会压实主题](ji-chu-zhi-shi/shen-ru-kafka/ya-shi/he-shi-hui-ya-shi-zhu-ti.md)
* [可靠的数据传递](ji-chu-zhi-shi/reliability/README.md)
  * [复制](ji-chu-zhi-shi/reliability/fu-zhi.md)
  * [broker配置](ji-chu-zhi-shi/reliability/broker-pei-zhi/README.md)
    * [复制系数](ji-chu-zhi-shi/reliability/broker-pei-zhi/fu-zhi-xi-shu.md)
    * [不彻底的首领选举](ji-chu-zhi-shi/reliability/broker-pei-zhi/bu-che-di-de-shou-ling-xuan-ju.md)
    * [最少同步副本](ji-chu-zhi-shi/reliability/broker-pei-zhi/zui-shao-tong-bu-fu-ben.md)
    * [保持副本同步](ji-chu-zhi-shi/reliability/broker-pei-zhi/bao-chi-fu-ben-tong-bu.md)
    * [持久化到磁盘](ji-chu-zhi-shi/reliability/broker-pei-zhi/chi-jiu-hua-dao-ci-pan.md)
  * [在可靠的系统中使用生产者](ji-chu-zhi-shi/reliability/zai-ke-kao-de-xi-tong-zhong-shi-yong-sheng-chan-zhe/README.md)
    * [发送确认](ji-chu-zhi-shi/reliability/zai-ke-kao-de-xi-tong-zhong-shi-yong-sheng-chan-zhe/fa-song-que-ren.md)
    * [配置生产者的重试参数](ji-chu-zhi-shi/reliability/zai-ke-kao-de-xi-tong-zhong-shi-yong-sheng-chan-zhe/pei-zhi-sheng-chan-zhe-de-zhong-shi-can-shu.md)
    * [额外的错误处理](ji-chu-zhi-shi/reliability/zai-ke-kao-de-xi-tong-zhong-shi-yong-sheng-chan-zhe/e-wai-de-cuo-wu-chu-li.md)
  * [在可靠的系统中使用消费者](ji-chu-zhi-shi/reliability/zai-ke-kao-de-xi-tong-zhong-shi-yong-xiao-fei-zhe/README.md)
    * [消费者的可靠性配置](ji-chu-zhi-shi/reliability/zai-ke-kao-de-xi-tong-zhong-shi-yong-xiao-fei-zhe/xiao-fei-zhe-de-ke-kao-xing-pei-zhi.md)
    * [手动提交偏移量](ji-chu-zhi-shi/reliability/zai-ke-kao-de-xi-tong-zhong-shi-yong-xiao-fei-zhe/shou-dong-ti-jiao-pian-yi-liang.md)
  * [验证系统可靠性](ji-chu-zhi-shi/reliability/yan-zheng-xi-tong-ke-kao-xing.md)
* [精确一次性语义](ji-chu-zhi-shi/exactly-once/README.md)
  * [幂等生产者](ji-chu-zhi-shi/exactly-once/mi-deng-sheng-chan-zhe/README.md)
    * [工作原理](ji-chu-zhi-shi/exactly-once/mi-deng-sheng-chan-zhe/gong-zuo-yuan-li.md)
    * [局限性](ji-chu-zhi-shi/exactly-once/mi-deng-sheng-chan-zhe/ju-xian-xing.md)
    * [如何使用幂等生产者](ji-chu-zhi-shi/exactly-once/mi-deng-sheng-chan-zhe/ru-he-shi-yong-mi-deng-sheng-chan-zhe.md)
  * [事务](ji-chu-zhi-shi/exactly-once/shi-wu/README.md)
    * [事务可以解决哪些问题](ji-chu-zhi-shi/exactly-once/shi-wu/shi-wu-ke-yi-jie-jue-na-xie-wen-ti.md)
    * [事务是如何保证精确一次性的](ji-chu-zhi-shi/exactly-once/shi-wu/shi-wu-shi-ru-he-bao-zheng-jing-que-yi-ci-xing-de.md)
    * [事务不能解决哪些问题](ji-chu-zhi-shi/exactly-once/shi-wu/shi-wu-bu-neng-jie-jue-na-xie-wen-ti.md)
    * [如何使用事务](ji-chu-zhi-shi/exactly-once/shi-wu/ru-he-shi-yong-shi-wu.md)
    * [事务 ID 和隔离](ji-chu-zhi-shi/exactly-once/shi-wu/shi-wu-id-he-ge-li.md)
    * [事务的工作原理](ji-chu-zhi-shi/exactly-once/shi-wu/shi-wu-de-gong-zuo-yuan-li.md)
    * [事务的性能](ji-chu-zhi-shi/exactly-once/shi-wu/shi-wu-de-xing-neng.md)

## Group 1

* [Listener](group-1/listener.md)
* [kafka面试题](group-1/review.md)
