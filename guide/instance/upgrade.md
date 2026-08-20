# 实例规格变更

实例规格支持在线扩容，可根据业务需要使用。

选择指定实例，在操作列选择“更改实例规格”按钮操作，如下:

![instance_upgrade_button](/rocketmq/images/service_upgrade_button.png)

根据业务需要调整至目标实例规格。

![instance_upgrade](/rocketmq/images/service_upgrade.png)


##  注意

- 4.x 暂不支持实例规格变更

- 实例扩容期间，可能产生以下影响：
    - 消息会重复
    - 延时消息会增大延迟
    - 分区顺序 Topic：
        - 选中的 Topic：队列数增加，可获得更高吞吐；队列数变化后，同一分区键可能映射到新队列，若仍有堆积，扩容后可能出现短暂乱序。
        - 未选择的 Topic：队列数保持不变，消息仍按原队列投递，分区内顺序不受影响，但吞吐提升有限。
    - 全局顺序 Topic 不保证顺序