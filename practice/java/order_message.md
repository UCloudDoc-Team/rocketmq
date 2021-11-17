# 收发顺序消息

RocketMQ提供严格按照顺序进行生产与消费的顺序消息类型。顺序消息包含全局顺序消息与分区顺序消息，说明如下：
* 全局顺序消息：所有消息保证FIFO生产和消费，实际为单队列。
* 分区顺序消息：利用用户提供Sharding Key进行区块分区，同一分区消息按照严格FIFO顺序进行发布与消费。

两类顺序消息生产与消费代码一致。

## 发送顺序消息

```
import java.io.UnsupportedEncodingException;
import java.util.List;
import org.apache.rocketmq.client.exception.MQBrokerException;
import org.apache.rocketmq.client.exception.MQClientException;
import org.apache.rocketmq.client.producer.DefaultMQProducer;
import org.apache.rocketmq.client.producer.MQProducer;
import org.apache.rocketmq.client.producer.MessageQueueSelector;
import org.apache.rocketmq.client.producer.SendResult;
import org.apache.rocketmq.common.message.Message;
import org.apache.rocketmq.common.message.MessageQueue;
import org.apache.rocketmq.remoting.common.RemotingHelper;
import org.apache.rocketmq.remoting.exception.RemotingException;
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
    public static void main(String[] args) throws UnsupportedEncodingException {
        try {
            // "ProducerGroupName"为生产组，用户可使用控制台创建的Group或者自定义
            DefaultMQProducer producer = new DefaultMQProducer("ProducerGroupName", getAclRPCHook());
            // 实例接入地址，可在实例列表页获取
            producer.setNamesrvAddr("1.1.1.1:9876");
            producer.start();

            String[] tags = new String[] {"TagA", "TagB", "TagC", "TagD", "TagE"};
            for (int i = 0; i < 100; i++) {
                int orderId = i % 10;
                Message msg =
                    new Message("Topic_Name", tags[i % tags.length], "Message_Key" + i,
                        ("Hello RocketMQ " + i).getBytes(RemotingHelper.DEFAULT_CHARSET));
                SendResult sendResult = producer.send(msg, new MessageQueueSelector() {
                    @Override
                    public MessageQueue select(List<MessageQueue> mqs, Message msg, Object arg) {
                        Integer id = (Integer) arg;
                        int index = id % mqs.size();
                        return mqs.get(index);
                    }
                }, orderId);

                System.out.printf("%s%n", sendResult);
            }

            producer.shutdown();
        } catch (MQClientException | RemotingException | MQBrokerException | InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

## 订阅顺序消息

```
import java.util.List;
import java.util.concurrent.atomic.AtomicLong;
import org.apache.rocketmq.client.consumer.DefaultMQPushConsumer;
import org.apache.rocketmq.client.consumer.listener.ConsumeOrderlyContext;
import org.apache.rocketmq.client.consumer.listener.ConsumeOrderlyStatus;
import org.apache.rocketmq.client.consumer.listener.MessageListenerOrderly;
import org.apache.rocketmq.client.consumer.rebalance.AllocateMessageQueueAveragely;
import org.apache.rocketmq.client.exception.MQClientException;
import org.apache.rocketmq.common.consumer.ConsumeFromWhere;
import org.apache.rocketmq.common.message.MessageExt;
import org.apache.rocketmq.acl.common.AclClientRPCHook;
import org.apache.rocketmq.acl.common.SessionCredentials;
import org.apache.rocketmq.remoting.RPCHook;

public class Consumer {
    // 实例接入使用公私钥，可在实例令牌管理页面获取
    private static final String AccessKey = "xxx";
    private static final String SecretKey = "xxx";

    static RPCHook getAclRPCHook() {
        return new AclClientRPCHook(new SessionCredentials(AccessKey, SecretKey));
    }

    public static void main(String[] args) throws MQClientException {
        // "GROUP_NAME"为消费组，可在实例Group管理页面获取
         DefaultMQPushConsumer consumer = new DefaultMQPushConsumer("GROUP_NAME", getAclRPCHook(), new AllocateMessageQueueAveragely());
        // 实例接入地址，可在实例列表页获取
        consumer.setNamesrvAddr("1.1.1.1:8100");
        consumer.subscribe("Topic_Name", "TagA || TagC || TagD");

        consumer.registerMessageListener(new MessageListenerOrderly() {
            AtomicLong consumeTimes = new AtomicLong(0);

            @Override
            public ConsumeOrderlyStatus consumeMessage(List<MessageExt> msgs, ConsumeOrderlyContext context) {
                context.setAutoCommit(true);
                System.out.printf("%s Receive New Messages: %s %n", Thread.currentThread().getName(), msgs);
                this.consumeTimes.incrementAndGet();
                if ((this.consumeTimes.get() % 2) == 0) {
                    return ConsumeOrderlyStatus.SUCCESS;
                } else if ((this.consumeTimes.get() % 3) == 0) {
                    return ConsumeOrderlyStatus.ROLLBACK;
                } else if ((this.consumeTimes.get() % 4) == 0) {
                    return ConsumeOrderlyStatus.COMMIT;
                } else if ((this.consumeTimes.get() % 5) == 0) {
                    context.setSuspendCurrentQueueTimeMillis(3000);
                    return ConsumeOrderlyStatus.SUSPEND_CURRENT_QUEUE_A_MOMENT;
                }

                return ConsumeOrderlyStatus.SUCCESS;
            }
        });

        consumer.start();
        System.out.printf("Consumer Started.%n");
    }
}
```