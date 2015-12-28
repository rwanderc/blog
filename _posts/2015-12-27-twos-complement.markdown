---
layout:      post
title:       "Two's Complement"
subtitle:    "how signed integer numbers are represented in binary"
description: "Briefly explanation of how signed integer numbers are represented in binary."
date:        2015-12-27 20:05:00
author:      "Wander Costa"
header-img:  "/img/post-bg-twos-complement.jpg"
thumb-img:   "/img/thumb/thumb-twos-complement.jpg"
---

Have you ever thought about how numbers are represented in your computer? We all know that computers "talk" in binary, but how exactly does it work?

In this post, I will briefly introduce **Two's Complement**, which is the way signed integer numbers are represented. Signed data type are data types that support representation of negative and positive values.

In the following examples, I will use Java 7 data types for explanations. Java 8 SE provides some differences in `int` types, that will not be considered.

## Limits

Data structures must have limits. And this is not only an old requirement, given the old computers capacities. Nowadays, with the amount of data introduced by BigData and many other high data usage scenarios, thinking about representation is reasonable to achieve good performance and capability.

This is one of the main reasons of the existence of different data types. Even with the current technologies and computer's capacities, resources are still not infinite. It's very common to have limits in representation, what means that you must choose the appropriate data type according to the problem being solved.

In Java, for example, the primitive type:

* `byte` is signed, 8 bits long ($$ -(2^{8}) $$ to $$ (2^{8} - 1) $$);
* `short` is signed, 16 bits long ($$ -(2^{16}) $$ to $$ (2^{16} - 1) $$);
* `int` is signed, 32 bits long ($$ -(2^{32}) $$ to $$ (2^{32} - 1) $$);
* `long` is signed, 64 bits long ($$ -(2^{64}) $$ to $$ (2^{64} - 1) $$).

C, however, has both signed and unsigned version of the above primitive types, and its `long` is 32 bits long.

## Understanding Representation

By default, integers `int` are signed 32 bits long, represented in two's complement, what means that it has the following limits:

* `0000 0000 0000 0000 0000 0000 0000 0000` represents `0`;
* `0111 1111 1111 1111 1111 1111 1111 1111` represents `2,147,483,647` ($$ 2^{31}-1 $$);
* `1000 0000 0000 0000 0000 0000 0000 0000` represents `-2,147,483,648` ($$ - (2^{32}) $$);
* `1111 1111 1111 1111 1111 1111 1111 1111` represents `-1`.

All the other three primitives that use Two's Complement work analogously.

The value 45, for example, may be represented in any of the mentioned types. However, in the background, the memory allocated for each data type  will be considerably different, because it will store:

* `0010 1101`, if the data type is `byte`;
* `0000 0000 0010 1101`, if the data type is `short`;
* `0000 0000 0000 0000 0000 0000 0010 1101` if the data type is `int`;
* `0000 0000 0000 0000 0000 0000 0000 0000 0000 0000 0000 0000 0000 0000 0010 1101` if the data type is `long`;

### Inferences

The left most bit (most significant bit) defines the signal. That is, it defines if the number is positive (`0`) or negative (`1`).

Considering `int` variables, the **binary sum** of `2,147,483,647` and `1` results `-2,147,483,648`. Considering `byte` variables, the binary sum of `127` and `1` results `-128`. Strange, nah?

Another inference that can be done is about mirroring. If you could fold the total length of the integer numbers in the middle, you would have:

* 0 mirroring -1;
* 1 mirroring -2;
* 2 mirroring -3, and so on, until;
* $$ (2^{31} - 1) $$ mirroring $$ - (2^{31}) $$.

**Curiosity:** Note that the individual sum of all mirrored values is -1.

## Sources

[The Java Tutorials - Primitive Data Type](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html)

[Tutorials Point - C Data Type](http://www.tutorialspoint.com/cprogramming/c_data_types.htm)
