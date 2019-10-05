---
layout:      post
title:       "The Decorator design pattern in software development"
subtitle:    "an easy understanding of the decorator design pattern and some examples"
description: "an easy understanding of the decorator design pattern and some examples"
date:        2019-09-21 09:20:00
author:      "Wander Costa"
header-img:  "/img/post-bg-pictureframes.jpg"
thumb-img:   "/img/post-thumb-pictureframes.jpg"
img-credits: "rawpixel.com"
img-credits-url: https://www.freepik.com/free-photos-vectors/frame
img-credits-real-url: https://www.freepik.com/free-psd/frames-living-room_4500896.htm#page=8&query=frames&position=39
tags:
- design pattern
- decorator pattern
- clean code
- software engineering
- software development
- development
hide:        "true"
redirect:    "/decorator-pattern"
---

### A decorator is like a picture frame. It wraps the main object, adding some other functionality to it.

<hr>

The main goal of the **Decorator** pattern is to enable the addition of functionalities or behaviors **dynamically** to objects.

To enable additional functionalities or behaviors in classes, a simple inheritance could easily do the job and no design pattern would be necessary. However, pay attention to the term "dynamically". It means **that the decorator must be instantiated after the original object already exists**, and therefore inheritance couldn't be the best solution.

In other words, a decorator usually receives the original object in its constructor and implements the same interfaces, adding some logic while calling the original object. The implementation of the same interfaces guarantees the transparency of the decorated object.

A great and good side effect of the pattern is that the additional functionalities provided by the decorator are of course implemented in other classes, avoiding adding complexity to the original class. This helps meet [SOLID's][solid] first principle, which is the [Single Responsibility Principle][singleresponsibility], by guaranteeing the separation of responsibilities and makes the classes easily testable.


# The Coffee Example

Suppose a **Coffee** interface and its concrete implementations **SimpleCoffee** and **Espresso**. The classes **CoffeeWithMilk** and **SecretRecipeCoffee** are decorators that will change behaviors by add ingredients and cost, or anything else.

![img/coffee-uml.png](/img/coffee-uml.png)

<script src="https://gist.github.com/rwanderc/3832b38283e9761ed22df12f7d69ee9a.js"></script>

The main difference in the implementation of the decorators and the other concrete classes is that the decorators wrap the original object while adding behavior.

<script src="https://gist.github.com/rwanderc/d5f4a5f1d5eb677c4d9f8601174d322c.js"></script>

And when executing the code below, it's easy to find how the behavior changed.

<script src="https://gist.github.com/rwanderc/a7552b9fdcb751d0405218ace66fe142.js"></script>

These code examples were simplified.

[solid]:https://en.wikipedia.org/wiki/SOLID
[singleresponsibility]:https://en.wikipedia.org/wiki/Single_responsibility_principle