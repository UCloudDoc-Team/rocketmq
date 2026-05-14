# Group创建

Group建议以"UGROUP-"为前缀，用户也可以自定义，实例内保持唯一，创建后名称不可修改。

RocketMQ 5.x 集群可指定消息投递的顺序，默认为并发投递。

选择顺序投递时，消息投递顺序与 RocketMQ 集群中队列存储的顺序一致。如需确保投递消息顺序与消息生产顺序一致，需要在创建 Topic 时指定消息类型为“顺序消息”。

![group_create](/rocketmq/images/group_create.png)