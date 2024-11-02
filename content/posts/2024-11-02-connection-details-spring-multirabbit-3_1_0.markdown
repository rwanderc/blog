---
layout: "post"
url: "/connection-details-spring-multirabbit-3.1.0/"
title: "ConnectionDetails in Spring Multirabbit 3.1.0"
subtitle: "ConnectionDetails was introduced in version 3.1.0, and here is how to use it"
date: "2024-11-02 13:59:15"
author: "Wander Costa"
tags:
  - software development
  - springboot
  - rabbitmq
  - spring multirabbit
  - open source

---

The main change introduced in Spring Multirabbit 3.1.0 was the support for `ConnectionDetails` ([GH-34657][gh-34657]).

`ConnectionDetails` was introduced in **Spring Boot 3.1.0** ([commit][connection-details-commit]) and enables Spring's
auto-configuration to utilize connection information from other sources than configuration properties. In other words,
it provides a way to overwrite some key properties for connections.
It has an impact not only on RabbitMQ connections but a bunch of other integrations like jdbc, mongo, kafka, liquibase,
cassandra, redis, elasticsearch, etc.

For the Rabbit use case, an object implementing the interface
`RabbitConnectionDetails` ([commit][rabbit-connection-details-commit]) is injected during the auto-configuration of the
default connection.
Or in case no bean is provided, Spring spins up an empty one and ignores the empty information in favor of the
configuration provided in properties.

For the Multirabbit use case, it would be necessary to have multiple `RabbitConnectionDetails`, one per multirabbit
connection in a way that they don't interfere with each other or with the default one.
Therefore, `MultiRabbitConnectionDetailsMap` ([link][mr-connection-details-map]) was created to work as a map of `RabbitConnectionDetails`,
and is also injected during auto-configuration.

The user willing to overwrite all the `RabbitConnectionDetails` needs to provide two beans, one
`RabbitConnectionDetails` and one `MultiRabbitConnectionDetailsMap`, and the multirabbit one needs to contain as many
entries, with matching keys, as configured in the multirabbit.
It would be something like:

```java
@Bean
RabbitConnectionDetails defaultRabbitConnectionDetails() {
    return ...
}

@Bean
MultiRabbitConnectionDetailsMap multiRabbitConnectionDetailsMap() {
    return ...
}
```

If the entries don't match the same multirabbit keys, they will be ignored. If the auto-configuration doesn't find the
matching `RabbitConnectionDetails` at `MultiRabbitConnectionDetailsMap`, the auto-configuration will spin up a new and
empty `RabbitConnectionDetails` instance, following the same logic as Spring's implementation.
In the end, every connection (including the default one) will have a `RabbitConnectionDetails` attached, either an empty
one provided by the auto-configuration or the one provided by the user via beans.

[gh-34657]: https://github.com/spring-projects/spring-boot/issues/34657
[connection-details-commit]: https://github.com/spring-projects/spring-boot/commit/aa91f2b8b67aa807437e373577d20db2ba12582e
[rabbit-connection-details-commit]: https://github.com/philwebb/spring-boot/commit/669aed786aef0c160fd6f653870c97d87311bea0
[mr-connection-details-map]: https://github.com/rwanderc/spring-multirabbit/blob/main/spring-multirabbit/src/main/java/org/springframework/boot/autoconfigure/amqp/MultiRabbitConnectionDetailsMap.java
