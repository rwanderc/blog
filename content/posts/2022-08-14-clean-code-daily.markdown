---
layout:      "post"
url:         "/clean-code-daily"
title:       "5 ways to write better code"
subtitle:    "5 pieces of advice that will help you write better code daily"
description: "5 pieces of advice that will help you write better code daily"
date:        "2022-08-14 17:30:00"
author:      "Wander Costa"
header_img:  "/img/post-bg-clean-code.jpg"
thumb_img:   "/img/post-bg-clean-code.jpg"
img_credits: "kues"
img_credits_url: https://www.freepik.com/author/kues
img_credits_real_url: https://www.freepik.com/free-photo/tree-beautiful-day_928890.htm
tags:
- code quality

---

Forget for a moment about correctness, efficiency, or performance. If your code has 10 nested `if`s or nested loops, the
computer will be able to process and run it the same way as if it had one or two. No matter how badly written is the
code, once it is compiled or interpreted without failure, the computer can run it.

However, developers should write code not only for computers to run but also for other developers to understand and
continue evolving it. Writing bad code will cause bad readability, slower evolution, and potential bugs.

> Decrease unnecessary complexity, and you will decrease the time to production. [Fetzner, Willian][linearb]

And if you are here, you might've heard the term **Clean Code**. When writing code, besides the obvious need to maximize
correctness, efficiency, and performance, there is a big need to keep code highly readable and maintainable.

It involves the design of the classes, methods, and functions; it has to do with code conventions, naming conventions,
the way the logic flow is organized, and design patterns being used; it has to do with how decoupled the code is; it has
to do with the maximization of simplicity, independently of how broad the requirements are. It's about writing a code
that you will read again in 6 months and understand what's going on. Not only you, but also other developers will be
able to read and understand, and easily continue evolving the code. This is called **Cognitive Complexity**.

> Cognitive Complexity is a measure of how difficult a unit of code is to intuitively understand. Unlike Cyclomatic
> Complexity, which determines how difficult your code will be to test, Cognitive Complexity tells you how difficult
> your code will be to read and understand. [CodeClimate][codeclimate]

This post is not intended to suggest to you Cognitive Complexity metrics to monitor or tools to use (maybe in another
post), or get deeper into this area of study. Instead, it's intended to give you actionable pieces of advice for you to
write better code every day.

Then, below my 5 most important aspects to write clean code daily:

# Use meaningful names

This is a basic rule that is often overlooked. Give variables, methods, and classes meaningful names.

Don't call a variable `sd` or `date1`; instead, call it `startDate` or `createDate`. Use names that would indicate
what's the purpose for the specific variable in the context of that piece of code. Collections can be named in the
plural or with the `list` or `set` suffix. E.g. `countries` or `countryList`.

The exception is the convention used for loop counters (e.g. `i` and `j`), but depending on the situation, I'd also
favor more comprehensive names. The same goes for method and class names. The next developer should be able to read the
code you wrote and connect the dots on what it's doing.

# Apply the Single Responsibility Principle

This is one of the most important aspects.

SRP is the `S` principle from [SOLID][solid] (and by the way, I recommend learning more about the 5 SOLID principles, as
they are very useful). And the idea behind SRP is that every class, module, or function should have a single
responsibility. If a method or class is covering many purposes, it might be a good indication that the code needs some
refactoring to break it down into more components.

Also, a very useful way to identify if a piece of code is doing too many things is to create unit tests. When it's hard
to create unit tests (e.g. due to the difficulty to mock internal behaviors) it can also be a good indicator of a highly
coupled code and that it might make sense to break it down into smaller and easier-to-test components. I use the time I
write tests (when not doing TDD) to refactor and untangle my implementation. Don't be afraid to refactor it.

# Shorten methods

This also has to do with the Single Responsibility principle. However, is more intended to make the blocks more concise
and independent. Keeping short methods or blocks make it easier for developers to understand the code, and connect these
understandings as components when reading it.

On the other hand, long methods might also be an indication of too many responsibilities. 

# Reduce flows and stick to the main one

Who never wrote that code that has 4 nested `if`s? 😬

Every `if` has 2 paths: the `condition = true` and the `condition = false`. Unit-testing code that has too many `if`s
reflects in having many different test cases. And obviously, it makes not only testing but reading and understanding
harder.

A personal approach I try to take as much as possible is to consider every `if` as a deviation of the regular flow, and
to write code in a way that the main flow is the one delivered at the end of the method.

Suppose a method to return if the client is senior (greater or equal to 60 years old) or to throw an exception with
negative input.

```java
boolean isSenior(int yearsOld) {
    if (yearsOld >= 60) {
        return true;
    } else if (yearsOld > 0) {
        return false;
    } else {
        throw new IllegalArgumentException("Negative age.");
    }
}
```

The example above is not wrong, but it's overcomplicated. You might think... why? It's only 7 lines long.

The reason is that the main flow (returning `true` for a senior) is under a deviation in the first `if`, which makes the
reader of the method lose focus on the purpose of the method right in the beginning. What if the method is re-written in
a way that returning `true` for a senior is the last thing done in the method? Let's rewrite it.

```java
boolean isSenior(int yearsOld) {
    if (yearsOld <= 0) {
        throw new IllegalArgumentException("Negative age.");
    } else {
        return yearsOld >= 60;
    }
}
```

And going further into simplifying it, what if the main flow is not even under an `if-else` block?

```java
boolean isSenior(int yearsOld) {
    if (yearsOld <= 0) {
        throw new IllegalArgumentException("Negative age.");
    }
    return yearsOld >= 60;
}
```

Now compare the 1st and 3rd examples and which one is easier to understand.

It might be too tiny readability optimization, but considering that developers have to read hundreds of lines of code
when working with more complex systems and that the real situations might be much more abstract than the example above,
everything that reduces the deviation of the reader's focus is a potentially good optimization.

# Ask for feedback from other developers

Unless you are working alone on a project and have nobody to discuss ideas or solutions (I completely do not recommend
such a thing), make sure you have someone else that can look at your code and opine, maybe via a pull request, maybe via
pairing session.

Different people, with different backgrounds and different experiences, have different ideas. Try to make use of this as
an opportunity to improve your code, learn or help someone else learn something new. That's an important part of the
work of a developer willing to grow in their career.

And enjoy the learnings!


[solid]: https://en.wikipedia.org/wiki/SOLID

[linearb]: https://linearb.io/blog/what-is-code-complexity/

[codeclimate]: https://docs.codeclimate.com/docs/cognitive-complexity#:~:text=Cognitive%20Complexity%20is%20a%20measure,be%20to%20read%20and%20understand.