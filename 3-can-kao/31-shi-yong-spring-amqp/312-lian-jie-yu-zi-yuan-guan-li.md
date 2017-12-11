**简介**

本文以RabbitMQ为例

管理RabbitMQ连接的核心组件是`ConnectionFactory`接口。该接口的职责是提供一个用于封装`com.rabbitmq.client.Connection`的`org.springframework.amqp.rabbit.connection.Connectiond`的实例。我们提供唯一一个具体的实现CachingConnectionFactory，默认情况下，该接口通过创建一个单连接的能够被应用程序共享的代理

