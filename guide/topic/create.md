# Topic创建

Topic建议以"UTOPIC-"为前缀，用户也可以自定义，实例内保持唯一，创建后名称不可修改。Topic支持不同消息类型，请按实际需要创建。

RocketMQ 5.x 集群会对消息类型进行强制校验，生产者发送的消息类型和 Topic 的消息类型不一致时，消息无法发送，集群会返回异常。

RocketMQ 4.x 集群没有消息类型的强制校验，但依然建议不要混用。

![topic_create](/rocketmq/images/topic_create.png)

## 轻量消息与优先级消息

`Lite` 和 `Priority` 类型 Topic 要求实例版本为 5.5.1 及以上，升级方式见 [实例版本升级](/rocketmq/guide/instance/version_upgrade)。

- 消息类型为 `Lite` 的 Topic 可配置 `LiteTopic` 的过期时间（分钟）：30～720。过期未收到新消息的 `LiteTopic` 会被自动清理；不过期仍受实例消息保留时间限制。