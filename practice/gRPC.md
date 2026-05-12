# gRPC 协议接入指南

5.x 集群通过 gRPC 协议接入，使用 [rocketmq-clients](https://github.com/apache/rocketmq-clients) 开源 Java SDK。以下按使用场景列出官方示例代码。

## 示例代码索引

| 场景分类 | 使用场景 | 示例名称 | 代码链接 |
| -------- | -------- | -------- | -------- |
| 生产者 | 发送普通消息 | ProducerNormalMessageExample | [ProducerNormalMessageExample.java](https://github.com/apache/rocketmq-clients/blob/master/java/client/src/main/java/org/apache/rocketmq/client/java/example/ProducerNormalMessageExample.java) |
| 生产者 | 发送顺序消息 | ProducerFifoMessageExample | [ProducerFifoMessageExample.java](https://github.com/apache/rocketmq-clients/blob/master/java/client/src/main/java/org/apache/rocketmq/client/java/example/ProducerFifoMessageExample.java) |
| 生产者 | 发送延迟消息 | ProducerDelayMessageExample | [ProducerDelayMessageExample.java](https://github.com/apache/rocketmq-clients/blob/master/java/client/src/main/java/org/apache/rocketmq/client/java/example/ProducerDelayMessageExample.java) |
| 生产者 | 发送事务消息 | ProducerTransactionMessageExample | [ProducerTransactionMessageExample.java](https://github.com/apache/rocketmq-clients/blob/master/java/client/src/main/java/org/apache/rocketmq/client/java/example/ProducerTransactionMessageExample.java) |
| 生产者 | 异步发送消息 | AsyncProducerExample | [AsyncProducerExample.java](https://github.com/apache/rocketmq-clients/blob/master/java/client/src/main/java/org/apache/rocketmq/client/java/example/AsyncProducerExample.java) |
| 消费者 | Push 模式消费 | PushConsumerExample | [PushConsumerExample.java](https://github.com/apache/rocketmq-clients/blob/master/java/client/src/main/java/org/apache/rocketmq/client/java/example/PushConsumerExample.java) |
| 消费者 | Simple 模式消费 | SimpleConsumerExample | [SimpleConsumerExample.java](https://github.com/apache/rocketmq-clients/blob/master/java/client/src/main/java/org/apache/rocketmq/client/java/example/SimpleConsumerExample.java) |
| 消费者 | 异步 Simple 消费 | AsyncSimpleConsumerExample | [AsyncSimpleConsumerExample.java](https://github.com/apache/rocketmq-clients/blob/master/java/client/src/main/java/org/apache/rocketmq/client/java/example/AsyncSimpleConsumerExample.java) |

## 依赖引入

Maven 项目在 `pom.xml` 中添加以下依赖：

```xml
<dependency>
    <groupId>org.apache.rocketmq</groupId>
    <artifactId>rocketmq-client-java</artifactId>
    <version>5.2.0</version>
</dependency>
```

## 接入参数说明

| 参数 | 获取方式 |
| ---- | -------- |
| 接入地址（endpoints） | 实例列表页获取，格式为 `ip:port` |
| Topic 名称 | Topic 管理页创建后获取 |
| Group 名称 | Group 管理页创建后获取 |
| 认证信息 | 令牌管理页获取公私钥 |
