**简介**

本文以RabbitMQ为例

管理RabbitMQ连接的核心组件是`ConnectionFactory`接口。该接口的职责是提供一个用于封装`com.rabbitmq.client.Connection`的`org.springframework.amqp.rabbit.connection.Connectiond`的实例。我们提供唯一一个具体的实现`CachingConnectionFactory`，默认情况下，该接口通过创建一个单连接的能够被应用程序共享的代理。连接实例提供一个`createChannel`方法，`CachingConnectionFactory`实现支持缓存这些通道，并且根据是否支持事务，为通道提供单独的缓存。当创建一个`CachingConnectionFactory`实例的时候，主机可以通过构造函数提供，包括用户名和密码。如果想要配置通道缓存的大小（默认25），可以调用`setChannelCacheSize()`方法。

在1.4.2版本，CachingConnectionFactory 有一个属性channelCheckoutTimeout，当它大于0时，表示通道的缓存达到限制

```
CachingConnectionFactory connectionFactory = new CachingConnectionFactory("somehost");
connectionFactory.setUsername("guest");
connectionFactory.setPassword("guest");

Connection connection = connectionFactory.createConnection();
```

使用XML表示：

```
<bean id="connectionFactory"
      class="org.springframework.amqp.rabbit.connection.CachingConnectionFactory">
    <constructor-arg value="somehost"/>
    <property name="username" value="guest"/>
    <property name="password" value="guest"/>
</bean>
```

ConnectionFactoru可以很方便的使用rabbit命名空间创建

```
<rabbit:connection-factory id="connectionFactory"/>
```

在大多数情况下，这种方式是种很好的选择，因为框架可以帮你选择合适的默认值。创建的实例是CachingConnectionFactory,请记住，默认的缓存大小是25，如果想要改变可以通过_channelCacheSize属性，如下：_

```
<bean id="connectionFactory"
      class="org.springframework.amqp.rabbit.connection.CachingConnectionFactory">
    <constructor-arg value="somehost"/>
    <property name="username" value="guest"/>
    <property name="password" value="guest"/>
    <property name="channelCacheSize" value="50"/>
</bean
```

使用命名空间表示

```
<rabbit:connection-factory
    id="connectionFactory" channel-cache-size="50"/>
```

默认的缓存模式是CHANNEL,但是可以配置成缓存连接connections

```
<rabbit:connection-factory
    id="connectionFactory" cache-mode="CONNECTION" connection-cache-size="25"/>
```

主机和端口属性也可以通过namespace配置

```
<rabbit:connection-factory
    id="connectionFactory" host="somehost" port="5672"/>
```

或者，如果在集群环境中运行，可以使用addresses属性

```
<rabbit:connection-factory
    id="connectionFactory" addresses="host1:5672,host2:5672"/>
```

下面是一个自定义的线程工厂例子，线程名使用rabbitmq-作为前缀

```
<rabbit:connection-factory id="multiHost" virtual-host="/bar" addresses="host1:1234,host2,host3:4567"
    thread-factory="tf"
    channel-cache-size="10" username="user" password="password" />

<bean id="tf" class="org.springframework.scheduling.concurrent.CustomizableThreadFactory">
    <constructor-arg value="rabbitmq-" />
</bean>
```

**配置底层客户端连接工厂**

CachingConnectionFactory使用Rabbit客户端ConnectionFactory的一个实例；当在CachingConnectionFactory配置等效属性时，通过一些配置属性（主机、端口、用户名、密码等）。配置其他属性时（例如clientProperties），可以定义一个rabbit工厂并且为其提供一个实现，通过使用适当的构造函数。

```
<rabbit:connection-factory
      id="connectionFactory" connection-factory="rabbitConnectionFactory"/>
```



