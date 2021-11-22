# 概览

## 客户端版本

推荐使用4.7.1版本SDK，可通过以下方式引入依赖：

### Maven引入

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


### 下载依赖包

可在RocketMQ官网下载：[Apache RocketMQ](https://rocketmq.apache.org/release_notes/release-notes-4.7.1)

## 接入代码参考

提供以下几种场景接入代码参考，更多请参考官方实例[Rocketmq Example](https://github.com/apache/rocketmq/tree/rocketmq-all-4.7.1/example/src/main/java/org/apache/rocketmq/example)

* [收发普通消息](/rocketmq/practice/java/normal_message)
* [收发顺序消息](/rocketmq/practice/java/order_message)
* [收发事务消息](/rocketmq/practice/java/transaction_message)
* [收发定时/延时消息](/rocketmq/practice/java/delay_message)
* [消息轨迹](/rocketmq/practice/java/message_trace)