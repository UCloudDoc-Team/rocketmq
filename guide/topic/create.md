# Topic创建

Topic建议以"UTOPIC-"为前缀，用户也可以自定义，实例内保持唯一，创建后名称不可修改。Topic支持不同消息类型，请按实际需要创建。

RocketMQ 5.x 集群会对消息类型进行强制校验，生产者发送的消息类型和 Topic 的消息类型不一致时，消息无法发送，集群会返回异常。

RocketMQ 4.x 集群没有消息类型的强制校验，但依然建议不要混用。

![topic_create](/rocketmq/images/topic_create.png)