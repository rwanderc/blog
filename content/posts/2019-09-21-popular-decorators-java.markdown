---
layout:      post
url:         "/popular-decorators-java/"
title:       "5 popular decorators from Java"
subtitle:    "5 popular decorators from Java you probably didn't even know they were decorators"
description: "5 popular decorators from Java you probably didn't even know they were decorators"
date:        2019-09-21 09:20:00
author:      "Wander Costa"
header_img:  "/img/post-bg-coffee-table.jpg"
thumb_img:   "/img/post-thumb-coffee-table.jpg"
img_credits: "jcomp"
img_credits_url: https://www.freepik.com/free-photos-vectors/background
img_credits_real_url: https://www.freepik.com/free-photo/hot-coffee-cup-set-wooden-table_4666781.htm#page=2&query=coffee&position=20
tags:
- java
- design patterns
---

As a software developer, you probably know the [Decorator Design Pattern][decorator-wiki], described in GoF. If not, [read this first][decorator].

Java has some nice decorators that can make your life easier, or tougher, depending on the case. Below, you will see a list of five decorators present in Java that you didn't even realize they were decorations.


## java.util.Collections.synchronizedList(List<T> list)

Java 8 provides many **synchronizedX()** methods, for lists, sets, maps, etc. They are analog.

The synchronizedList() will turn the access to the list **mutually exclusive**. That is, two threads will not have simultaneous access to any method of the object as a lock is held by the first thread. If **thread1** enters the **get()** method, **thread2** will be blocked to enter the method **size()** until **thread1** releases the lock.

For the concurrency concern, though, there is a better solution. For Maps, for example, **ConcurrentHashMap** is more performant. As described for lists, the analog method **synchronizedMap()** will turn all methods **mutually exclusive**, even the methods that only read data. In other hands, **ConcurrentHashMap** avoids blocking the object for reads and provides a more fine-grained lock for writes. This makes **ConcurrentHashMap** more performant then **synchronizedMap()** and should be preferred.


## java.util.Collections.unmodifiableList(List<T> list)

As well as for synchronizedList() method, Java 8 provides analog **unmodifiableX()** methods for lists, sets, maps, etc.

The decoration here is that the list cannot be changed. Therefore, the methods that would change the list throw an **UnsupportedOperationException**.

There are some discussions if that would be the best design to provide immutable collections, but this post does not cover this discussion.


## java.util.Collections.checkedList(List<T> list, Class<T> type)

This was introduced in Java 5, as well as [Generics][generics]. This method decorates the list returning a dynamic typesafe view of the list. That is, in each insertion, it will throw a **ClassCastException** if the type of the item added does not match the type provided for the decorator.

Stepping back a little, a better code, not runtime-dependent, could be achieved with the use of Generics. That's right!

But this decorator can be a second option if eventually there are legacy code involved, in which changing the construction of the object is not an option.


## java.io.BufferedReader

Yeah, every Java developer had once to use input or output streams to read or write data from/to streams, files, etc. This is one of the many decorators in the io package. As per the javadocs, BufferedReader:

> Reads text from a character-input stream, buffering characters so as to provide for the efficient reading of characters, arrays, and lines.

The BufferedReader provides the Reader functionality and adds on the top of that the methods to retrieve complete lines instead of single characters. Thus, it simplifies a lot the work to read text files.

Java 8 introduced the concept of Streams in the language, and also the method **lines()** in the BufferedReader. This is easy to use and fully compatible with streams.

{{< gist rwanderc 022176bf021a4118bd2efdc7b788c714 >}}


## java.util.zip.GZIPInputStream

The GZIPInputStream is one of the many decorators of InputStream, that adds the functionality of reading compressed data in GZIP file format.

{{< gist rwanderc 8cc4e5aa275573d2520a12ed9511cc35 >}}


---


For more tutorials from Java collections and their decorators, please visit [Wrapper Implementations][wrapper-impl].


[decorator-wiki]:https://en.wikipedia.org/wiki/Decorator_pattern
[decorator]:/the-decorator-design-pattern-in-software-development/
[generics]:https://docs.oracle.com/javase/tutorial/java/generics/types.html
[wrapper-impl]:https://docs.oracle.com/javase/tutorial/collections/implementations/wrapper.html