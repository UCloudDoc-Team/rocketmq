# 接入指引

新用户可以按照以下步骤完成URocketMQ产品快速接入使用：

## 创建实例

创建需要规格的实例，并获取实例的接入地址。

## 创建Topic

在“Topic管理”页创建指定消息类型的Topic。

## 创建Group

在“Group管理”页创建Group。

## 获取ACL

在“概览”页ACL模块获取实例访问认证信息。

注意，该ACL信息为URocketMQ自定义的SDK接入认证信息，并非用户产品API访问公私钥。

## SDK接入

URocketMQ兼容开源RocketMQ协议，用户可下载对应语言的开源SDK，并利用实例接入地址、Topic名称、Group名称、ACL信息进行使用。

说明：JAVA SDK版本建议不低于4.7.1。
