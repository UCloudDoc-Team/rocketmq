# 消费模型

URocketMQ 普通 Topic 支持 `CLUSTERING` 与 `BROADCASTING` 两种消费模型；绑定 `Lite` 类型 Topic 的 Group 支持 `LITE_SELECTIVE` 与 `CLUSTERING`。

## CLUSTERING

`CLUSTERING` 下相同消费组内的消费者实例平均分摊消息，每条消息仅会被其中一个消费者消费。

绑定 `Lite` 类型 Topic 的 Group 使用 `CLUSTERING` 时，组内消费者共享该 Topic 下所有 LiteTopic 的消息，不支持排他消费。

## BROADCASTING

`BROADCASTING` 下相同消费组下的消费者都会接收所有的消息，每条消息会被所有消费者消费。

注意事项：

1. `BROADCASTING` 下消费进度在客户端维护，出现重复消费的概率高于 `CLUSTERING`，同时控制台不支持该模式下的消息堆积查看及相关告警。
2. `BROADCASTING` 下不支持顺序消费。
3. `BROADCASTING` 下不支持重置消息位。

基于以上几点，建议用户优先使用 `CLUSTERING` 消费。

## LITE_SELECTIVE

绑定 `Lite` 类型 Topic 的 Group 默认使用 `LITE_SELECTIVE`，由客户端选择订阅指定的 LiteTopic。开启排他消费后，同一个 LiteTopic 仅允许一个消费者独占消费。