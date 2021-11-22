# 消息轨迹

业务使用消息轨迹需要在生产侧与消费侧都开启消息轨迹。

## 生产侧开启

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
    private static final String ACCESS_KEY = "xxx";
    private static final String SECRET_KEY = "xxx";

    static RPCHook getAclRPCHook() {
        return new AclClientRPCHook(new SessionCredentials(ACCESS_KEY, SECRET_KEY));
    }
    public static void main(String[] args) throws MQClientException, InterruptedException {

        // "ProducerGroupName"为生产组，用户可使用控制台创建的生产Group或者自定义，最后一个参数设置为""
        DefaultMQProducer producer = new DefaultMQProducer("ProducerGroupName", getAclRPCHook(), true, "");
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

## 消费侧开启

```
import java.util.List;
import org.apache.rocketmq.client.consumer.DefaultMQPushConsumer;
import org.apache.rocketmq.client.consumer.listener.ConsumeConcurrentlyContext;
import org.apache.rocketmq.client.consumer.listener.ConsumeConcurrentlyStatus;
import org.apache.rocketmq.client.consumer.listener.MessageListenerConcurrently;
import org.apache.rocketmq.client.consumer.rebalance.AllocateMessageQueueAveragely;
import org.apache.rocketmq.client.exception.MQClientException;
import org.apache.rocketmq.common.message.MessageExt;
import org.apache.rocketmq.acl.common.AclClientRPCHook;
import org.apache.rocketmq.acl.common.SessionCredentials;
import org.apache.rocketmq.remoting.RPCHook;

public class PushConsumer {
    // 实例接入使用公私钥，可在实例令牌管理页面获取
    private static final String ACCESS_KEY = "xxx";
    private static final String SECRET_KEY = "xxx";

    static RPCHook getAclRPCHook() {
        return new AclClientRPCHook(new SessionCredentials(ACCESS_KEY, SECRET_KEY));
    }

    public static void main(String[] args) throws InterruptedException, MQClientException {
        // "GROUP_NAME"为消费组，可在实例Group管理页面获取，最后一个参数设置为""
         DefaultMQPushConsumer consumer = new DefaultMQPushConsumer("GROUP_NAME", getAclRPCHook(),
            new AllocateMessageQueueAveragely(), true, "");
        // 实例接入地址，可在实例列表页获取
        consumer.setNamesrvAddr("1.1.1.1:8100");
        consumer.subscribe("Topic_Name", "*");
        consumer.registerMessageListener(new MessageListenerConcurrently() {

            @Override
            public ConsumeConcurrentlyStatus consumeMessage(List<MessageExt> msgs, ConsumeConcurrentlyContext context) {
                System.out.printf("%s Receive New Messages: %s %n", Thread.currentThread().getName(), msgs);
                return ConsumeConcurrentlyStatus.CONSUME_SUCCESS;
            }
        });
        consumer.start();
        System.out.printf("Consumer Started.%n");
    }
}
```

