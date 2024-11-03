---
layout: "post"
url: "/retry-logic-spring-multirabbit/"
title: "Retry logic in Spring Multirabbit"
subtitle: "Retry logic for RabbitMQ listeners in Spring Boot"
date: "2024-11-03 02:09:15"
author: "Wander Costa"
tags:
  - software development
  - springboot
  - rabbitmq
  - spring multirabbit
  - open source

---

Some days ago I realized `spring-multirabbit` had a bug that affected the retry support since its first version.
I was browsing for `spring-multirabbit` in stackoverflow and found this [question][sof-question] reporting the issue
which was also linked to the original [issue][fn-issue] that I took but never had the time to check. Thank you folks
that opened the question and issue.

I just launched the following `spring-multirabbit` versions that fix it:

- [v3.3.1][v3.3.1]
- [v3.2.1][v3.2.1]
- [v3.1.7][v3.1.7]
- [v3.0.2][v3.0.2]
- [v2.7.4][v2.7.4]
- [v2.6.3][v2.6.3]

And here is how to use it.
Suppose three brokers, where the first is the default configuration under `spring.rabbitmq` and there are two other
brokers, configured under `broker1` and `broker2`.
There is no inheritance logic for the retry logic, therefore every broker needs to be configured accordingly.

```properties
spring.rabbitmq.listener.simple.retry.enabled = true
spring.rabbitmq.listener.simple.retry.initial-interval = 1000
spring.rabbitmq.listener.simple.retry.max-attempts = 5

spring.multirabbitmq.enabled = true
spring.multirabbitmq.connections.broker1.listener.simple.retry.enabled = true
spring.multirabbitmq.connections.broker1.listener.simple.retry.initial-interval = 2000
spring.multirabbitmq.connections.broker1.listener.simple.retry.max-attempts = 10

spring.multirabbitmq.connections.broker2.listener.simple.retry.enabled = true
spring.multirabbitmq.connections.broker2.listener.simple.retry.initial-interval = 5000
spring.multirabbitmq.connections.broker2.listener.simple.retry.max-attempts = 20
```

In the example configuration, the default connection is configured with 5 retries with 1sec interval, while `broker1` is
configured with 10 retries with 2sec interval and `broker2` is configured with 20 retries with 5sec interval.

[sof-question]: https://stackoverflow.com/questions/77035681/listener-goes-into-infinite-retries-when-using-multirabbit-connection
[fn-issue]: https://github.com/freenowtech/spring-multirabbit/issues/73
[v3.3.1]: https://github.com/rwanderc/spring-multirabbit/releases/tag/spring-multirabbit-parent-3.3.1
[v3.2.1]: https://github.com/rwanderc/spring-multirabbit/releases/tag/spring-multirabbit-parent-3.2.1
[v3.1.7]: https://github.com/rwanderc/spring-multirabbit/releases/tag/spring-multirabbit-parent-3.1.7
[v3.0.2]: https://github.com/rwanderc/spring-multirabbit/releases/tag/spring-multirabbit-parent-3.0.2
[v2.7.4]: https://github.com/rwanderc/spring-multirabbit/releases/tag/spring-multirabbit-parent-2.7.4
[v2.6.3]: https://github.com/rwanderc/spring-multirabbit/releases/tag/spring-multirabbit-parent-2.6.3
