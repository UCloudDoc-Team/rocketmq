# Group创建

Group建议以"UGROUP-"为前缀，用户也可以自定义，实例内保持唯一，创建后名称不可修改。

RocketMQ 5.x 集群可指定消息投递的顺序，默认为并发投递。

选择顺序投递时，消息投递顺序与 RocketMQ 集群中队列存储的顺序一致。如需确保投递消息顺序与消息生产顺序一致，需要在创建 Topic 时指定消息类型为“顺序消息”。

![group_create](/rocketmq/images/group_create.png)

## 绑定 Lite 类型 Topic

RocketMQ 5.x 集群在创建 Group 时可绑定一个 `Lite` 类型的 Topic，绑定关系在创建时确定。支持以下两种消费模式：

- `LITE_SELECTIVE`：默认模式，由客户端按需选择订阅的 `LiteTopic`，同一 Group 内不同消费者的订阅关系可以不一致，可开启排他消费（开启后同一个 `LiteTopic` 仅允许一个消费者独占消费）。
- `CLUSTERING`：组内消费者共享消费绑定 Topic 下所有 `LiteTopic` 的消息，不支持排他消费。

对于绑定了 Lite 类型 Topic 的 Group 选择并发投递时, 批次内并发投递, 批次间有序。选择顺序投递时,每个 LiteTopic 的消息按照顺序投递。

![group_create_lite](/rocketmq/images/group_create_lite.png)