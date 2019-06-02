---
layout:      post
title:       "Lombok Constructors"
subtitle:    "easy constructors with lombok"
description: "easy constructors with lombok"
date:        2017-05-06 01:56:00
author:      "Wander Costa"
header-img:  "/img/post-bg-lombok.jpg"
thumb-img:   "/img/thumb/thumb-lombok.jpg"
tags:
- constructor
- productivity
---

In the post [Lombok Getters and Setters][gettersandsetters], we talked about the use of Lombok to provide quick and easy getters and setters.

In this post, I will cover another great feature from Lombok, that provides quick and easy constructors. For this post, I will use Lombok `v1.16.16`.<!--more-->

In Lombok, we have 3 different annotations to deal with constructors:

 * **NoArgsConstructor**: mostly used to create a constructor with different access modification, preventing it from being accessed from other classes;
 * **AllArgsConstructor**: will create constructor receiving all non-static fields;
 * **RequiredArgsConstructor**: will create constructor receiving non-static final fields.

We are mostly interested in the second and third items, that are more commonly used.

## AllArgsConstructor
The `@AllArgsConstructor` annotation creates the constructor with all the fields as parameters. In this example, it's exemplified in the static block.
<script src="https://gist.github.com/rwanderc/23ede9af395f427a210e276f4250f597.js"></script>

It's also possible to change the constructor's access modifier. In the following example, it's providing `protected` access modifier, that will prevent accesses from classes that are not inheriting from the specified class.
<script src="https://gist.github.com/rwanderc/743a4c6e7035a72aae0b7d28010495e1.js"></script>

## RequiredArgsConstructor
The `@RequiredArgsConstructor` works the same way as `@AllArgsConstructor`. However, it only considers the fields that are non-static and final. It guarantees that the fields covered will not be changed after construction, once it requires them to be final.
<script src="https://gist.github.com/rwanderc/8d297819793e2d810af024c38fcac011.js"></script>

## Conclusion
Once more, we use Lombok to reduce boilerplate code in our applications. We can make use of these features to increase the readability of our classes.

[gettersandsetters]:http://www.wandercosta.com/lombok/
