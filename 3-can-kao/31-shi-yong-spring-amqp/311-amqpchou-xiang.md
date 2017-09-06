**引言**

Spring AMQP由几个用JAR表示的模块组成。这些模块是：spring-amqp和spring-rabbit。spring-amqp包含 `org.springframework.amqp.core`包，在这个包中，你会发现有很多核心的“模型”，我们的目的是提供通用的抽象，也就是不依赖特定的AMQP代理实现和客户端库。由于只针对抽象层开发，最终用户编写的代码具有更好的移植性。这些抽象会被特殊的代理实现，如spring-rabbit。目前来说只有一个RabbitMQ的实现。Spring AMQP主要工作在协议层，而RabbitMQ客户端提供同样的协议层被使用。

**Message**

Spring AMQP定义了消息类，作为AMQP域模型的一部分。消息类的目的主要是将消息主体与属性封装在一个实例中，简化API。消息类的定义非常简单。

```
public class Message {

	private final MessageProperties messageProperties;
	
	private final byte\[\] body;
	
	public Message\(byte\[\] body, MessageProperties messageProperties\) {
	
	this.body = body;
	
	this.messageProperties = messageProperties;
	
	}

	public byte\[\] getBody\(\) {
	
	return this.body;
	
	}
	
	public MessageProperties getMessageProperties\(\) {
	
	return this.messageProperties;
	
	}
}
```

