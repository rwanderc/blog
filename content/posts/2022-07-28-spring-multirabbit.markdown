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

In 2019, I published this article **[SpringBoot with Multiple RabbitMQ brokers][oldpost]** with the first version
of `spring-multirabbit`. However, it hasn't been updated for some time, and many things changed since then. This post is
intended to be updated from time to time whenever there are changes in the library.

This post is revised until **SpringBoot 2.7.2**.

---

SpringBoot doesn't offer a very easy way of handling multiple RabbitMQ servers without introducing the complexity of
SpringCloud Stream or the need to manually provide multiple connection factories in code.

The **spring-multirabbit** library solves this missing feature, requiring none or minimal changes to the code and very
simple extension of the configuration.

# How to use?

## Dependencies

Add `spring-boot-starter-amqp` and `spring-multirabbit`.

The correct version of `spring-multirabbit` depends on the version of SpringBoot. For more information about versioning,
read below.

```xml
<dependency>
    <groupId>com.mytaxi.spring.multirabbit</groupId>
    <artifactId>spring-multirabbit-lib</artifactId>
    <version>${multirabbit.version}</version>
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


[oldpost]: https://medium.com/inside-freenow/springboot-with-multiple-rabbitmq-brokers-cec203c3f77

---