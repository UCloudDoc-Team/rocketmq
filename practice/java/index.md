# 概览

## 客户端版本

4.x 版本推荐使用4.7.1版本SDK，5.x 版本推荐使用5.4.0版本SDK，可通过以下方式引入依赖：

### Maven引入

- 4.x版本
```
<dependency>
    <groupId>org.apache.rocketmq</groupId>
    <artifactId>rocketmq-client</artifactId>
    <version>4.7.1</version>
</dependency>
<dependency>
    <groupId>org.apache.rocketmq</groupId>
    <artifactId>rocketmq-acl</artifactId>
    <version>4.7.1</version>
</dependency>
```

- 5.x版本
```
<dependency>
    <groupId>org.apache.rocketmq</groupId>
    <artifactId>rocketmq-client</artifactId>
    <version>5.4.0</version>
</dependency>
<dependency>
    <groupId>org.apache.rocketmq</groupId>
    <artifactId>rocketmq-acl</artifactId>
    <version>5.3.2</version>
</dependency>
```

### 下载依赖包

可在RocketMQ官网下载：

- 4.x版本：[Apache RocketMQ](https://rocketmq.apache.org/release-notes/2020/06/29/4.7.1/)
- 5.x版本：[Apache RocketMQ](https://rocketmq.apache.org/release-notes/2025/12/24/5.4.0/)

## 接入代码参考

提供以下几种场景接入代码参考，更多请参考官方实例

- 4.x版本：[Rocketmq Example](https://github.com/apache/rocketmq/tree/rocketmq-all-4.7.1/example/src/main/java/org/apache/rocketmq/example)
- 5.x版本：[Rocketmq Example](https://github.com/apache/rocketmq/tree/rocketmq-all-5.4.0/example/src/main/java/org/apache/rocketmq/example)

* [收发普通消息](/rocketmq/practice/java/normal_message)
* [收发顺序消息](/rocketmq/practice/java/order_message)
* [收发事务消息](/rocketmq/practice/java/transaction_message)
* [收发延时消息](/rocketmq/practice/java/delay_message)
* [消息轨迹](/rocketmq/practice/java/message_trace)
