# 迁移指南

如果您已使用其他消息队列产品，可按照以下步骤迁移至URocketMQ使用。


## 已使用开源RocketMQ SDK

如果您使用消息队列产品为RocketMQ并使用开源RocketMQ相关SDK，那么仅需更改少许配置即可接入，具体步骤如下:

1. 创建实例并创建同名Topic、Group。
    * [创建实例](/rocketmq/guide/instance/create)
    * [创建Topic](/rocketmq/guide/topic/create)
    * [创建Group](/rocketmq/guide/group/create)
2. 确认SDK版本，如Java建议使用4.7.1版本或以上。
3. SDK修改如下配置信息
   * 接入地址：可在[实例列表](/rocketmq/guide/instance/list)获取
   * 令牌公私钥：可在[令牌列表](/rocketmq/guide/token)获取

通过以上3个简单步骤即可完成迁移接入。

## 未使用开源RocketMQ SDK

如果您使用消息队列产品为非RocketMQ类别或者使用其他途径SDK，需要更改业务代码，可按照以下步骤接入：

1. 创建实例并创建同名Topic、Group；
    * [创建实例](/rocketmq/guide/instance/create)
    * [创建Topic](/rocketmq/guide/topic/create)
    * [创建Group](/rocketmq/guide/group/create)
2. 根据开发语言选择对应的SDK，具体见[接入指南](/rocketmq/practice/index)
3. SDK接入客户端程序，需要配置信息如下:
    * 接入地址：可在[实例列表](/rocketmq/guide/instance/list)获取
    * 令牌公私钥：可在[令牌列表](/rocketmq/guide/token)获取
    * Topic：创建Topic名称
    * Group：创建Group名称