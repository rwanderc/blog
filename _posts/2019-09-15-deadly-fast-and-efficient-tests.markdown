---
layout:      post
title:       "4 rules to write deadly fast and efficient unit tests in Java with Mockito"
subtitle:    "If your unit tests are taking long to run, you're doing it wrong!"
description: "4 rules to write deadly fast and efficient unit tests in Java with Mockito"
date:        2019-09-15 22:00:00
author:      "Wander Costa"
header-img:  "/img/post-bg-clock.jpg"
thumb-img:   "/img/post-thumb-clock.jpg"
img-credits: "rawpixel.com"
img-credits-url: https://www.freepik.com/free-photos-vectors/businesss
img-credits-real-url: https://www.freepik.com/free-vector/devices_2945063.htm#page=1&query=clock&position=1
tags:
- unit tests
- unit
- test
- java
- software development
- development
---

**Unit testing** in [OOP][OOP] is the test of the smallest testable piece of code: the class. It's used to make sure a single class does what it's supposed to do.

After some years of experience, I got to see unit testing as a crucial part of the software development process, since they give me some important advices that I couldn't identify before writing them. Not being able to write a good and simple unit test might be a sign of code smell, advising you that your code is too coupled or is being responsible for too many things. **Time for refactoring!**

I'm used to following 4 big rules for unit tests:
- MUST be isolated
- MUST be independent among them
- MUST run quickly
- MUST cover edge cases

For the following sections, suppose you are implementing a simplified checkout basket for a shopping website. Product is a simple interface, which will be implemented in the future by another developer.

<script src="https://gist.github.com/rwanderc/597cdb8312ea2acaaed1c24a9105fe27.js"></script>


# Isolation

Unit tests must run isolated: **a test of class A cannot depend on the behavior of class B**. However, there are indeed cases where the functionality of a class depends on the behavior of another. In these cases, it's important to understand the responsibility of class A and test only that.

That's what happens with **CheckoutBasket**. How to isolate CheckoutBasket and test **totalPrice()** without depending on any concrete implementation of **Product**?

An awesome (I'd say indispensable) approach for isolation is the use of [mocks](mocks). In Java, [Mockito](mockito) is one of the libraries for it.

<script src="https://gist.github.com/rwanderc/29d26d5e9268a9da3c6853423530b914.js"></script>

As you can see, Mockito can provide a mock of the interface Product. With **Mockito.when(p1.getPrice()).thenReturn(productPrice)** we define the behavior we would expect from a Product when its method **getPrice()** is called. However, we don't have any concrete implementation of Product and indeed it's not even necessary.


# Independency

Unit tests must be independent of each other. **Sharing memory between tests is tricky and can lead to mistakes or inconsistencies.**

<script src="https://gist.github.com/rwanderc/d3e4ed009a9d1fcb8d63b10ef87e2220.js"></script>

As you see, the basket is static, therefore shared among any execution of the test. The first test runs successfully alone but fails when running with the other since they depend on the same basket instance. Whenever possible, avoid shared memory. If extremely necessary, adopt the practice of clearing up before each test.

Another aspect of the independency is that each test must test a single thing. It's often translated as: one test does only one assertion. And therefore you might have multiple tests for the same method, focusing on different aspects of the behavior of this method.


# Speedy

In order to produce deadly fast tests, you must avoid overly complex or bloated configuration, for example by loading Spring context with @RunWith(SpringJUnit4ClassRunner.class). Unit tests must run quickly and they should be easily repeatable. **Avoid loading contexts!**

There are scenarios, however, where the developer must load some context for testing. If that's the case, **IT'S NOT A UNIT TEST**, but an integration test.

Contexts are heavy to be loaded, almost always depend on some configuration and can be tricky to control. Unit testing is not intended to cover any of these concerns.


# Edge Cases

Since unit tests are the quicker tests you will have in your system, **try to cover as many edge cases as possible with unit tests**. Otherwise, they will eventually appear as integration tests or (worse) as bugs.

In the CheckoutBasket example, what if the **add()** method is called with a null Product? Well, from the logic perspective, it makes no sense to have a null item. But, as per javadocs for [ArrayLists](arraylist), the list will accept null values. Therefore it will only fail late when calling the **totalPrice()** when iterating on the list.

That's a good candidate for refactoring, by choosing a data structure that would not accept null items, or by null-checking the Product before adding to the list. Below, the second option:

<script src="https://gist.github.com/rwanderc/aa9f83bef492af0a89c203c6d34ce0d1.js"></script>

And eventually, a test can be introduced to guarantee this behavior.

<script src="https://gist.github.com/rwanderc/ec7127753a15efa17a27655ee06eb4f2.js"></script>

Null-checks in Java are not the only edge cases that exist. All other possible inputs that might be tricky for the logic implemented should be tested: parameters that are not expected as zero or negative; objects holding some inner attributes that are used; positions in an array; values used in a division; etc. Identifying them and refining the logic is the developer's job.



<hr>

This post roughly describes my main concerns and approach when creating unit tests. However, there are many other decisions I take on a daily basis to chose what and how to test. Maybe in other posts, I can bring some others.

And what are your tips to write awesome tests? Do you have any? Comment below.


[oop]:https://en.wikipedia.org/wiki/Object-oriented_programming
[mocks]:https://en.wikipedia.org/wiki/Mock_object
[mockito]:https://site.mockito.org/
[arraylist]:https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html