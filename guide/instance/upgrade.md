# 实例规格变更

实例规格支持在线扩容，可根据业务需要使用。

选择指定实例，在操作列选择“更改实例规格”按钮操作，如下:

![instance_upgrade_button](/rocketmq/images/service_upgrade_button.png)

根据业务需要调整至目标实例规格。

![instance_upgrade](/rocketmq/images/service_upgrade.png)


##  注意

- 4.x 暂不支持实例规格变更

- 扩容可选的的实例规格会随 commitlog 存储占比增大而减少

- 当实例规格变更时，不支持对实例进行其余破坏性操作