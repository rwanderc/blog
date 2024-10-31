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
I decided to branch it and continue supporting it myself under my [github.com/rwanderc/spring-multirabbit][gh] fork and
releasing under my [wandercosta.com maven namespace][maven].

I created this project at FREE NOW in 2019 when I was working there, however I stepped away from its evolution while in
the company due to the focus in other areas, and even further when I left the company in 2023.

However, apparently there are some users of this library, as people has being asking for the support of further
SpringBoot versions.
Recently, in October 2024, I re-branched the project under my GitHub account and started working on the compatibility
updates from Spring Boot 2.7.x until 3.4.x (current last version). Check the [compatibility list][gh-compatibility] for
details.

Current last version is `com.wandercosta.multirabbit:spring-multirabbit:3.3.0` for compatibility with SpringBoot 3.3.x.

```xml
<dependency>
    <groupId>com.wandercosta.multirabbit</groupId>
    <artifactId>spring-multirabbit</artifactId>
    <version>3.3.0</version>
</dependency>
```

However, I'm not a user of this library any longer️ :grimacing:, as I don't have such use case in my daily routine. This
means that I might not easily capture nuances of the evolution needs of the library, and need your help to keep evolving
it.
In resume, I'm welcoming contributions, issues, comments, pull requests, and every way you can contribute to the
evolution of the library at [github.com/rwanderc/spring-multirabbit][gh].

[fn-gh]: https://github.com/freenowtech/spring-multirabbit
[fn-mvn]: https://mvnrepository.com/artifact/com.free-now.multirabbit/spring-multirabbit
[gh]: https://github.com/rwanderc/spring-multirabbit
[maven]: https://mvnrepository.com/artifact/com.wandercosta.multirabbit/spring-multirabbit-parent
[gh-compatibility]: https://github.com/rwanderc/spring-multirabbit/wiki#compatibility