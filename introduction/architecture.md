
# 产品架构

产品架构如下

![architecture](/rocketmq/images/architecture.png)

* NameServer：路由注册中心，提供Broker管理与路由信息管理功能。

* Broker：消息处理模块，负责消息的存储、投递、查询等功能，包含Broker Master与Broker Slave两种角色，单个Master节点可对应多个Slave节点。

* Producer：消息生产者，支持集群部署。

* Consumer：消息消费者，支持集群部署，支持pull与push两种方式。

# 5.x 版本架构升级

1. 新增 Proxy 集群：弹性无状态客户端代理服务。拆分了 Broker 组件中的客户端协议适配、权限管理、消费管理等计算逻辑，实现存算分离。增加 gRPC 协议支持，兼容原有的 Remoting 协议。
2. 新增 Controller 组件：实现 Broker 集群自动主从切换。
