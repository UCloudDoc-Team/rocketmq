
# 产品架构

产品架构如下

![architecture](/rocketmq/images/architecture.png)

* NameServer：路由注册中心，提供Broker管理与路由信息管理功能。

* Broker：消息处理模块，负责消息的存储、投递、查询等功能，包含Broker Master与Broker Slave两种角色，单个Master节点可对应多个Slave节点。

* Producer：消息生产者，支持集群部署。

* Consumer：消息消费者，支持集群部署，支持pull与push两种方式。

* 高可用模块：UCloud自研Broker可用性管理模块，监测主从Broker节点可用性，在Broker Master节点异常时可实现自主主从切换，保证集群可用。

