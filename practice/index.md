# 接入指南

新用户可以按照以下步骤完成URocketMQ产品快速接入使用：

## 创建实例

创建需要配置的实例，可在实例列表获取实例访问地址，具体参考[实例创建](/rocketmq/guide/instance/create.md)。

## 创建Topic

在“Topic管理”页创建指定消息类型的Topic并获取Topic名称，具体参考[Topic创建](/rocketmq/guide/topic/create.md)。

## 创建Group

在“Group管理”页创建Group并获取Group名称，具体参考[Group创建](/rocketmq/guide/group/create.md)。

## 获取令牌公私钥

在“令牌管理”页选择对应的令牌获取公钥与私钥，客户端接入时需要使用，具体参考[令牌列表](/rocketmq/guide/token)。

## SDK接入

URocketMQ兼容开源RocketMQ协议，用户可下载对应语言的开源SDK，并利用实例接入地址、Topic名称、Group名称、令牌公私钥信息进行接入。

不同语言SDK接入实例如下，请根据实际情况选择不同语言SDK及版本。
* [JAVA接入指南](/rocketmq/practice/java/index)
* [Golang Example](https://github.com/apache/rocketmq-client-go/tree/v2.1.1-rc2/examples)
* [C++ Example](https://github.com/apache/rocketmq-client-cpp/tree/master/example)
* [Python Example](https://github.com/apache/rocketmq-client-python/tree/master/samples)
* [NodeJS Example](https://github.com/apache/rocketmq-client-nodejs)