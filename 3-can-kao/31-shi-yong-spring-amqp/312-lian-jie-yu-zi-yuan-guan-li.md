**简介**

本文以RabbitMQ为例

管理RabbitMQ连接的核心组件是`ConnectionFactory`接口。该接口的职责是提供一个用于封装`com.rabbitmq.client.Connection`的`org.springframework.amqp.rabbit.connection.Connectiond`的实例。我们提供唯一一个具体的实现CachingConnectionFactory，默认情况下，该接口通过创建一个单连接的能够被应用程序共享的代理。连接实例提供一个createChannel方法，CachingConnectionFactory实现支持缓存这些通道，并且根据是否支持事务，为通道提供单独的缓存。当创建一个CachingConnectionFactory实例的时候，主机可以通过构造函数提供，包括用户名和密码。如果想要配置通道缓存的大小（默认25），可以调用setChannelCacheSize\(\)方法。

