# 发送信息

可点击“发送信息”按钮并填写相关信息发送该消息，该功能可按实际需要使用。

![topic_send_message](/rocketmq/images/topic_send_message.png)

## 发送轻量消息

- 轻量消息需指定 `Lite` 类型的父 Topic 和目标 `LiteTopic`，其他类型 Topic 不支持设置 `LiteTopic`。
- `LiteTopic` 名称长度为 1～64 个字符，仅支持英文字母（`a-z`、`A-Z`）、数字（`0-9`）、下划线（`_`）和短横线（`-`），不能使用系统保留名称或前缀。首次发送或订阅不存在的 `LiteTopic` 时由服务端按需自动创建。

![topic_send_lite_message](/rocketmq/images/topic_send_lite_message.png)
