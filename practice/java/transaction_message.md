# 收发事务消息

应用本地事务和发送消息操作可以被定义到全局事务中，要么同时成功，要么同时失败。

## 发送事务消息

```
import org.apache.rocketmq.client.exception.MQClientException;
import org.apache.rocketmq.client.producer.SendResult;
import org.apache.rocketmq.client.producer.TransactionListener;
import org.apache.rocketmq.client.producer.TransactionMQProducer;
import org.apache.rocketmq.common.message.Message;
import org.apache.rocketmq.remoting.common.RemotingHelper;
import java.io.UnsupportedEncodingException;
import java.util.concurrent.ArrayBlockingQueue;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.ThreadFactory;
import java.util.concurrent.ThreadPoolExecutor;
import java.util.concurrent.TimeUnit;
import org.apache.rocketmq.acl.common.AclClientRPCHook;
import org.apache.rocketmq.acl.common.SessionCredentials;
import org.apache.rocketmq.remoting.RPCHook;

public class TransactionProducer {
    // 实例接入使用公私钥，可在实例令牌管理页面获取
    private static final String AccessKey = "xxx";
    private static final String SecretKey = "xxx";

    static RPCHook getAclRPCHook() {
        return new AclClientRPCHook(new SessionCredentials(AccessKey, SecretKey));
    }
    public static void main(String[] args) throws MQClientException, InterruptedException {
        TransactionListener transactionListener = new TransactionListener() {
            @Override
            public LocalTransactionState executeLocalTransaction(Message message, Object o) {
                System.out.println("执行本地事务");
                return LocalTransactionState.COMMIT_MESSAGE;
            }

            @Override
            public LocalTransactionState checkLocalTransaction(MessageExt messageExt) {
                System.out.println("回查事务状态");
                return LocalTransactionState.COMMIT_MESSAGE;
            }
        };

        // "ProducerGroupName"为生产组，用户可使用控制台创建的Group或者自定义
        TransactionMQProducer producer = new TransactionMQProducer("ProducerGroupName",  getAclRPCHook());
        ExecutorService executorService = new ThreadPoolExecutor(2, 5, 100, TimeUnit.SECONDS, new ArrayBlockingQueue<Runnable>(2000), new ThreadFactory() {
            @Override
            public Thread newThread(Runnable r) {
                Thread thread = new Thread(r);
                thread.setName("client-transaction-msg-check-thread");
                return thread;
            }
        });

        producer.setExecutorService(executorService);
        // 实例接入地址，可在实例列表页获取
        producer.setNamesrvAddr("1.1.1.1:9876");
        producer.setTransactionListener(transactionListener);
        producer.start();

        String[] tags = new String[] {"TagA", "TagB", "TagC", "TagD", "TagE"};
        for (int i = 0; i < 10; i++) {
            try {
                Message msg =
                    new Message("Topic_Name", tags[i % tags.length], "KEY" + i,
                        ("Hello RocketMQ " + i).getBytes(RemotingHelper.DEFAULT_CHARSET));
                SendResult sendResult = producer.sendMessageInTransaction(msg, null);
                System.out.printf("%s%n", sendResult);

                Thread.sleep(10);
            } catch (MQClientException | UnsupportedEncodingException e) {
                e.printStackTrace();
            }
        }

        for (int i = 0; i < 100000; i++) {
            Thread.sleep(1000);
        }
        producer.shutdown();
    }
}
```

其中executeLocalTransaction与checkLocalTransaction为生产者需要实现的两个回调函数，说明如下:
* executeLocalTransaction: 执行本地事务逻辑；
* checkLocalTransaction: 服务端如果一直收不到提交或者回滚请求，就会向客户端发起回查，客户端接受回查请求检查本地事务状态重新发送事务提交或者回滚请求。

两个回调函数均可能返回以下三种事务状态：

* LocalTransactionState.COMMIT_MESSAGE：提交事务，允许订阅方消费该消息；
* LocalTransactionState.ROLLBACK_MESSAGE：回滚事务，消息将被丢弃不允许消费；
* LocalTransactionState.UNKNOW：未知状态，服务端会向生产者再次回查该消息的状态；