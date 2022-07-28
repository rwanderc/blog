---
layout:      "post"
url:         "/sprint-multirabbit/"
title:       "Spring Multirabbit"
subtitle:    "SpringBoot with multiple RabbitMQ servers"
date:        "2022-07-28 09:09:00"
author:      "Wander Costa"
tags:

- software development
- messaging
- springboot
- rabbitmq
- multirabbit

---

In 2019, I published this article [**SpringBoot with Multiple RabbitMQ brokers**][oldpost] with the first version
of `spring-multirabbit`. This post is intended to be an up-to-date version of that one whenever there are changes in the
library.

Last revision: **SpringBoot 2.7.2** & `spring-multirabbit:2.7.0`.

---

SpringBoot doesn't offer a very easy way of handling multiple RabbitMQ servers without introducing the complexity of
SpringCloud Stream or the need to manually provide multiple connection factories in code.

The `spring-multirabbit` library solves this problem, requiring none or minimal changes to the code and very simple
extension of the configuration.

# How to use?

## Dependencies

Add `spring-boot-starter-amqp` and `spring-multirabbit`.

```xml

<dependency>
    <groupId>com.mytaxi.spring.multirabbit</groupId>
    <artifactId>spring-multirabbit</artifactId>
    <version>2.7.0</version>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-amqp</artifactId>
</dependency>
```

Update the configuration to introduce the additional servers.

The example below has a default server configured at `spring.rabbitmq` and introduces two additional servers configured
under `spring.multirabbitmq.serverB` and `spring.multirabbitmq.thirdServer`.

```properties
spring.rabbitmq.host:10.0.0.10
spring.rabbitmq.port:5672
spring.multirabbitmq.serverB.host:msgbroker.somedomain.com
spring.multirabbitmq.serverB.port:5672
spring.multirabbitmq.thirdServer.host:173.49.20.18
spring.multirabbitmq.thirdServer.port:5672
```

For the sake of simplification, no credentials or any other configuration is being provided. However, all the possible
parameters that are available under `spring.rabbitmq` are available for the additional servers.

### Versions

The correct version of `spring-multirabbit` depends on the version of SpringBoot:

- SpringBoot 2.7.x -> `spring-multirabbit:2.7.0`
- SpringBoot 2.6.x -> `spring-multirabbit:2.6.0`
- SpringBoot 2.5.x -> `spring-multirabbit:2.5.0`

There are some cases where SpringBoot's APIs change which requires another `spring-multirabbit` version, however this is
very unlikely to happen. More info regarding compatibility in the [project's wiki page][compatibility].

## Listeners

The `spring-multirabbit` library creates necessary beans using the same names as the servers provided from the
configuration. Thus, you can start listening to messages from the specified server by simply setting
`containerFactory=[servername]`.

```java
@RabbitListener(queues = "someQueue")
void listenToDefaultServer(...){...} // Listening to server serverB

@RabbitListener(queues = "someQueueName", containerFactory = "serverB")
void listenToServerB(...){...} // Listening to server thirdServer

@RabbitListener(queues = "anotherQueueName", containerFactory = "thirdServer")
void listenToThirdServer(...){...}
```

In the above example, the 3 servers are being listened: the default one (which does not require any binding), serverB
and thirdServer.

## RabbitTemplate

To send or receive messages using `RabbitTemplate`, it’s necessary to indicate the context, done programmatically
with [`SimpleResourceHolder`][srh]. This class is able to bind the specific connection factory to the running thread.
It’s necessary to bind and unbind it afterward, so the thread does not keep the reference.

```java
@Autowired
private RabbitTemplate template;

void sendMessageToDefaultServer(){
    template.convertAndSend(...);
}

void sendMessageToServerB(){
    SimpleResourceHolder.bind(template.getConnectionFactory(),"serverB");
    template.convertAndSend(...);
    SimpleResourceHolder.unbind(template.getConnectionFactory());
}

void sendMessageToThirdServer(){
    SimpleResourceHolder.bind(template.getConnectionFactory(),"thirdServer");
    template.convertAndSend(...);
    SimpleResourceHolder.unbind(template.getConnectionFactory());
}
```

Note that it would be a good practice to have a `try-catch` block to unbind it inside the `finally` block, but for the
simplification, that was not done in the example.

Yet for the simplification, `spring-multirabbit` brings a utility class for context handling. The class
`ConnectionFactoryContextWrapper` is provided as a bean and wraps the logic of binding and unbinding the connection
factory while providing a simple way to execute a `Runnable` or `Callable`. The example above would then be rewritten as
below:

```java
@Autowired
private RabbitTemplate template;

@Autowired
private ConnectionFactoryContextWrapper contextWrapper;

void sendMessageToDefaultServer(){
    template.convertAndSend(...);
}

void sendMessageToServerB(){
    contextWrapper.run("serverB",()->template.convertAndSend(...));
}

void sendMessageToThirdServer(){
    contextWrapper.run("thirdServer",()->template.convertAndSend(...));
}
```

# More examples

For more examples, access the [example modules][examples] in the source code.

# Source Code

`spring-multirabbit` is open source and available at https://github.com/freenowtech/spring-multirabbit. Feel free to
contribute.


[oldpost]: https://medium.com/inside-freenow/springboot-with-multiple-rabbitmq-brokers-cec203c3f77

[srh]: https://docs.spring.io/spring-amqp/api/org/springframework/amqp/rabbit/connection/SimpleResourceHolder.html

[compatibility]: https://github.com/freenowtech/spring-multirabbit/wiki

[examples]: https://github.com/freenowtech/spring-multirabbit/tree/main/spring-multirabbit-examples
