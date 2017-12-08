**引言**

Spring AMQP由几个用JAR表示的模块组成。这些模块是：spring-amqp和spring-rabbit。spring-amqp包含 `org.springframework.amqp.core`包，在这个包中，你会发现有很多核心的“模型”，我们的目的是提供通用的抽象，也就是不依赖特定的AMQP代理实现和客户端库。由于只针对抽象层开发，最终用户编写的代码具有更好的移植性。这些抽象会被特殊的代理实现，如spring-rabbit。目前来说只有一个RabbitMQ的实现。Spring AMQP主要工作在协议层，而RabbitMQ客户端提供同样的协议层被使用。

**Message**

Spring AMQP定义了消息类，作为AMQP域模型的一部分。消息类的目的主要是将消息主体与属性封装在一个实例中，简化API。消息类的定义非常简单。

```
public class Message {

    private final MessageProperties messageProperties;

    private final byte [] body;

    public Message (byte [] body, MessageProperties messageProperties) {

        this.body = body;

        this.messageProperties = messageProperties;

    }

    public byte [] getBody() {

        return this.body;

    }  


    public MessageProperties getMessageProperties() {

        return this.messageProperties;

    }
}
```

`MessageProperties`接口定义了几个通用的属性，例如 _messageId_、_timestamp_、_contentType_等，这些属性也可以通过调用`setHeader(String key, Object value)`方法进行自定义`headers`。

**Exchange**

`Exchange`接口代表了一个AMQP Exchange，消息生产者所发送的地方。一个虚拟主机中的每个Exchange都有一个唯一的名称和其他一些属性：

```
public interface Exchange {

    String getName();

    String getExchangeType();

    boolean isDurable();

    boolean isAutoDelete();

    Map<String, Object> getArguments();

}
```

通过上面可以发现，Exchange有一个`type`,基本的类型有：`Direct`、`Topic`、`Fanout`、`Header`，在核心包中会有针对每种类型的具体实现

**Queue**

`Queue`代表了消费者接收消息的的组件来源，与大多数的Exchange类似，我们的目的是为了实现AMQP的抽象表示

```
public class Queue {

    private final String name;

    private volatile boolean durable; 

    private volatile boolean exclusive;

    private volatile boolean autoDelete;

    private volatile Map<String, Object> arguments;

    /**
     * The queue is durable, non-exclusive and non auto-delete.
     *
     * @param name the name of the queue.
     */
    public Queue(String name) {
        this(name, true, false, false);
    } 
}
```



