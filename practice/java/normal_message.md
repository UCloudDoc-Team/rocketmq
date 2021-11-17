# 收发普通消息

## 发送普通消息

普通消息支持同步发送与异步发送两种发送方式，用户可根据业务场景选择。

### 同步发送

同步发送是指客户端发送一条消息到服务端，等待服务端响应之后再发送下一条消息。

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

        // "ProducerGroupName"为生产组，用户可使用控制台创建的Group或者自定义
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

### 异步发送

异步发送是指客户端发送一条消息之后，不等待服务端返回就发送小一条消息。


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

        // "ProducerGroupName"为生产组，用户可使用控制台创建的Group或者自定义
        DefaultMQProducer producer = new DefaultMQProducer("ProducerGroupName", getAclRPCHook());
        // 实例接入地址，可在实例列表页获取
        producer.setNamesrvAddr("1.1.1.1:9876");
        producer.start();

        for (int i = 0; i < 128; i++)
            try {
                {
                    Message msg = new Message("Topic Name",
                        "Message_Tag",
                        "Message_Key",
                        "Message Content Hello World".getBytes(RemotingHelper.DEFAULT_CHARSET));
                   producer.send(msg, new SendCallback() {
                    @Override public void onSuccess(SendResult result) {
                        // 发送成功
                        System.out.println("send message success");
                    }

                    @Override public void onException(Throwable throwable) {
                        // 发送失败，可重新发送或者存储该条消息进行补偿
                        System.out.println("send message failed.");
                        throwable.printStackTrace();
                    }
                });
                }

            } catch (Exception e) {
                e.printStackTrace();
            }

        producer.shutdown();
    }
}
```

## 订阅普通消息

```
import java.util.List;
import org.apache.rocketmq.client.consumer.DefaultMQPushConsumer;
import org.apache.rocketmq.client.consumer.listener.ConsumeConcurrentlyContext;
import org.apache.rocketmq.client.consumer.listener.ConsumeConcurrentlyStatus;
import org.apache.rocketmq.client.consumer.listener.MessageListenerConcurrently;
import org.apache.rocketmq.client.consumer.rebalance.AllocateMessageQueueAveragely;
import org.apache.rocketmq.client.exception.MQClientException;
import org.apache.rocketmq.common.consumer.ConsumeFromWhere;
import org.apache.rocketmq.common.message.MessageExt;
import org.apache.rocketmq.acl.common.AclClientRPCHook;
import org.apache.rocketmq.acl.common.SessionCredentials;
import org.apache.rocketmq.remoting.RPCHook;

public class PushConsumer {
    // 实例接入使用公私钥，可在实例令牌管理页面获取
    private static final String AccessKey = "xxx";
    private static final String SecretKey = "xxx";

    static RPCHook getAclRPCHook() {
        return new AclClientRPCHook(new SessionCredentials(AccessKey, SecretKey));
    }

    public static void main(String[] args) throws InterruptedException, MQClientException {
        // "GROUP_NAME"为消费组，可在实例Group管理页面获取
         DefaultMQPushConsumer consumer = new DefaultMQPushConsumer("GROUP_NAME", getAclRPCHook(), new AllocateMessageQueueAveragely());
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

