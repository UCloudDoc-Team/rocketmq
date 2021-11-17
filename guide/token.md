# 令牌管理

URocketMQ以令牌的形式提供实例访问机制，用户访问实例必须使用令牌。每个令牌可以Topic纬度设置生产与消费权限，以满足多个用户使用同一个实例不同Topic需求。

每个令牌包括生产与消费两种权限，具体如下：
* 全部：对实例下所有的Topic均有生产或者消费权限，包括后续创建的Topic。
* 部分：对指定选择的Topic有生产或者消费权限。
* 无：对实例下所有Topic均为生产或者消费权限。

实例创建会自动生成一个默认“Default”令牌，享有所有Topic生产与消费权限，仅支持查看，不允许编辑与删除。

## 令牌列表

可点击指定令牌的公私钥后的复制按钮后去公私钥完整信息。

![token_list](/rocketmq/images/token_list.png)

## 创建令牌

如果在默认Default令牌无法满足业务需求，可按需创建新的令牌。

点击”创建令牌“按钮创建新的令牌，可按需配置生产与消费权限。

![token_create](/rocketmq/images/token_create.png)

## 查看/编辑令牌

点击”查看/编辑”按钮可查看或者更新已有的令牌，其中默认Default令牌仅支持查看。

![token_edit](/rocketmq/images/token_edit.png)

## 删除令牌

可按需删除不需要的令牌，其中默认Default令牌不支持删除操作。

说明：删除令牌后，使用该令牌访问的客户端将无法再继续访问实例。

![token_delete](/rocketmq/images/token_delete.png)