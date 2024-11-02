---
layout: "post"
url: "/spring-multirabbits-in-2024/"
title: "Spring Multirabbit's update in 2024"
subtitle: "Updates on Spring Multirabbit in 2024 and further"
date: "2024-10-31 20:46:21"
author: "Wander Costa"
tags:
  - software development
  - springboot
  - rabbitmq
  - spring multirabbit
  - open source

---

[**spring-multirabbit**][fn-gh] ([maven][fn-mvn]) project was discontinued in February 2024 and FREE NOW will not
maintain it any longer.
I decided to fork it and continue supporting it myself under my [github.com/rwanderc/spring-multirabbit][gh] repository
and releasing under my [wandercosta.com maven namespace][maven].

**DISCLAIMER: The library and this initiative to continue supporting it are completely independent of my previous or
current employers.**

I created this project in 2019 while working at FREE NOW, but I stepped away from its evolution yet in the company due
to the focus change towards engineering management, and even further when I left the company in 2023.

After I stopped evolving it, some people kept asking about the maintenance of the library, requesting it to support
further versions of Spring Boot. Apparently, it's useful to some folks.
Recently, in October 2024, I forked the project under my GitHub account and started working on the compatibility
updates from Spring Boot 2.7.x until 3.3.x (current version 3.3.5). Check the
[compatibility list][gh-compatibility] for further details.

Current version is `com.wandercosta.multirabbit:spring-multirabbit:3.3.0` for compatibility with SpringBoot 3.3.x.

```xml
<dependency>
    <groupId>com.wandercosta.multirabbit</groupId>
    <artifactId>spring-multirabbit</artifactId>
    <version>3.3.0</version>
</dependency>
```

It's important to mention as well that I'm not a user of this library any longer️ :grimacing:, as I don't have such a use
case in my daily routine.
It means that I might not easily capture nuances of the evolution needs of the library, and need your help to keep
evolving it.
In other words, I'm welcoming contributions, issues, comments, pull requests, and every way you can contribute to the
evolution of the library at [github.com/rwanderc/spring-multirabbit][gh].

[fn-gh]: https://github.com/freenowtech/spring-multirabbit
[fn-mvn]: https://mvnrepository.com/artifact/com.free-now.multirabbit/spring-multirabbit
[gh]: https://github.com/rwanderc/spring-multirabbit
[maven]: https://mvnrepository.com/artifact/com.wandercosta.multirabbit/spring-multirabbit-parent
[gh-compatibility]: https://github.com/rwanderc/spring-multirabbit/wiki#compatibility