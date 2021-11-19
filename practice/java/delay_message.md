# 定时/延时消息

定时/延时消息是指消息发送到服务端后，不会立即被消费，等待特定时间投递给真正的topic。

URocketMQ提供两种定时/延时机制：
* 开源固定梯度延时：开源RocketMQ支持固定梯度的延迟消息，消息发送到之后，不会立即被消费，等待特定时间投递给真正的topic，默认配置“1s 5s 10s 30s 1m 2m 3m 4m 5m 6m 7m 8m 9m 10m 20m 30m 1h 2h” 18个级别，每个级别对应不同的延时时间，比如1对应1s，5对应1m
* 自定义定时/延时：URocketMQ自研支持任意秒级延迟消息，用户可自定义定时或者延迟至未来任意一秒发送消息，使用限制参见[消息类型](rocketmq/introduction/features/message_type)。

定时/延时消息仅在生产侧需要处理，消费侧代码无需特殊处理。

## 生产定时/延时消息

### 开源固定梯度延时

可根据实际需求设置不同的延迟等级，如下:

```
import org.apache.rocketmq.client.exception.MQClientException;
import org.apache.rocketmq.client.producer.DefaultMQProducer;
import org.apache.rocketmq.client.producer.SendResult;
import org.apache.rocketmq.common.message.Message;
import org.apache.rocketmq.remoting.common.RemotingHelper;
import org.apache.rocketmq.acl.common.AclClientRPCHook;
import org.apache.rocketmq.acl.common.SessionCredentials;
import org.apache.rocketmq.remoting.RPCHook;

public class Producer {
    // 实例接入使用公私钥，可在实例令牌管理页面获取
    private static final String AccessKey = "xxx";
    private static final String SecretKey = "xxx";

    static RPCHook getAclRPCHook() {
        return new AclClientRPCHook(new SessionCredentials(AccessKey, SecretKey));
    }
    public static void main(String[] args) throws MQClientException, InterruptedException {

        // "ProducerGroupName"为生产组，用户可使用控制台创建的生产Group或者自定义
        DefaultMQProducer producer = new DefaultMQProducer("ProducerGroupName", getAclRPCHook());
        // 实例接入地址，可在实例列表页获取
        producer.setNamesrvAddr("1.1.1.1:9876");
        producer.start();

        for (int i = 0; i < 128; i++)
            try {
                {
                    Message msg = new Message("Topic_Name",
                        "Message_Tag",
                        "Message_Key",
                        "Message Content Hello World".getBytes(RemotingHelper.DEFAULT_CHARSET));
                    // 可按需设置不同延迟级别
                    msg.setDelayTimeLevel(3);
                    SendResult sendResult = producer.send(msg);
                    System.out.printf("%s%n", sendResult);
                }

            } catch (Exception e) {
                e.printStackTrace();
            }

        producer.shutdown();
    }
}
```

### 自定义定时/延时

自定义定时与延时代码稍有区别，具体如下:

#### 自定义定时消息

```
import java.text.SimpleDateFormat;
import org.apache.rocketmq.client.exception.MQClientException;
import org.apache.rocketmq.client.producer.DefaultMQProducer;
import org.apache.rocketmq.client.producer.SendResult;
import org.apache.rocketmq.common.message.Message;
import org.apache.rocketmq.remoting.common.RemotingHelper;
import org.apache.rocketmq.acl.common.AclClientRPCHook;
import org.apache.rocketmq.acl.common.SessionCredentials;
import org.apache.rocketmq.remoting.RPCHook;

public class Producer {
    // 实例接入使用公私钥，可在实例令牌管理页面获取
    private static final String AccessKey = "xxx";
    private static final String SecretKey = "xxx";

    static RPCHook getAclRPCHook() {
        return new AclClientRPCHook(new SessionCredentials(AccessKey, SecretKey));
    }
    public static void main(String[] args) throws MQClientException, InterruptedException {

        // "ProducerGroupName"为生产组，用户可使用控制台创建的生产Group或者自定义
        DefaultMQProducer producer = new DefaultMQProducer("ProducerGroupName", getAclRPCHook());
        // 实例接入地址，可在实例列表页获取
        producer.setNamesrvAddr("1.1.1.1:9876");
        producer.start();

        // 支持秒级单位延迟，startDeliverTime单位为毫秒
        long startDeleverTime = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").parse("2021-11-01 11:49:00").getTime();
        for (int i = 0; i < 128; i++)
            try {
                {
                    Message msg = new Message("Topic_Name",
                        "Message_Tag",
                        "Message_Key",
                        "Message Content Hello World".getBytes(RemotingHelper.DEFAULT_CHARSET));
                    msg.putUserProperty("__STARTDELIVERTIME", String.valueOf(startDeliverTime));
                    SendResult sendResult = producer.send(msg);
                    System.out.printf("%s%n", sendResult);
                }

            } catch (Exception e) {
                e.printStackTrace();
            }

        producer.shutdown();
    }
}
```

#### 自定义延时消息

```
import java.text.SimpleDateFormat;
import org.apache.rocketmq.client.exception.MQClientException;
import org.apache.rocketmq.client.producer.DefaultMQProducer;
import org.apache.rocketmq.client.producer.SendResult;
import org.apache.rocketmq.common.message.Message;
import org.apache.rocketmq.remoting.common.RemotingHelper;
import org.apache.rocketmq.acl.common.AclClientRPCHook;
import org.apache.rocketmq.acl.common.SessionCredentials;
import org.apache.rocketmq.remoting.RPCHook;

public class Producer {
    // 实例接入使用公私钥，可在实例令牌管理页面获取
    private static final String AccessKey = "xxx";
    private static final String SecretKey = "xxx";

    static RPCHook getAclRPCHook() {
        return new AclClientRPCHook(new SessionCredentials(AccessKey, SecretKey));
    }
    public static void main(String[] args) throws MQClientException, InterruptedException {

        // "ProducerGroupName"为生产组，用户可使用控制台创建的生产Group或者自定义
        DefaultMQProducer producer = new DefaultMQProducer("ProducerGroupName", getAclRPCHook());
        // 实例接入地址，可在实例列表页获取
        producer.setNamesrvAddr("1.1.1.1:9876");
        producer.start();

        for (int i = 0; i < 128; i++)
            try {
                {
                    Message msg = new Message("Topic_Name",
                        "Message_Tag",
                        "Message_Key",
                        "Message Content Hello World".getBytes(RemotingHelper.DEFAULT_CHARSET));
                    long currMillseconds = System.currentTimeMillis();
                    // 支持秒级单位延迟，startDeliverTime单位为毫秒
                    long startDeliverTime = currMillseconds + (i % timeDuration) * 1000;
                    msg.putUserProperty("__STARTDELIVERTIME", String.valueOf(startDeliverTime));
                    SendResult sendResult = producer.send(msg);
                    System.out.printf("%s%n", sendResult);
                }

            } catch (Exception e) {
                e.printStackTrace();
            }

        producer.shutdown();
    }
}
```

## 订阅定时/延时消息

订阅定时/延时消息与订阅普通消息一致，具体可参考[订阅普通消息](./normal_message)。
