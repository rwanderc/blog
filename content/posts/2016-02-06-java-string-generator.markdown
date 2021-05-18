---
shorturl:    "http://goo.gl/PAZt9P"
layout:      post
url:         "/java-string-generator/"
title:       "Simple String Generator"
subtitle:    "how to generate random strings programmatically"
description: "how to generate random strings programmatically"
date:        2016-02-06 18:00:00
author:      "Wander Costa"
header_img:  "/img/post-bg-programming.jpg"
thumb_img:   "/img/post-thumb-programming.jpg"
tags:
- java
---

[github]:https://github.com/rwanderc
[git-stringgenerator]:https://github.com/rwanderc/examples/tree/master/string-generator

It's usually necessary to have a way to produce random strings programmatically:

* Produce random IDs for entities;
* Produce random passwords;
* Produce random patterns, etc.

Today, I will show you a very quick way to create those strings for studying purpose or as a simple solution.<!--more-->

However, __I do really recommend__ you to use `RandomStringUtils` API from Apache library `commons-lang3` to use in your projects for production. This library covers many possibilities and you will avoid adding any unnecessary class into your project. The library can be added into your `pom.xml` with the following dependency.

``` xml
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-lang3</artifactId>
    <version>3.4</version>
    <type>jar</type>
</dependency>
```

Back to this simple project, it was compiled using Java 8, but you can compile also in previous versions of Java (maybe some adjustments will be necessary).

This [code][git-stringgenerator] is also available in my <i class="fa fa-github"></i> [GitHub][github] page. Feel free to comment if you have any doubt or suggestion. :smile:

### Licensing

This software is licensed under the MIT license, what basically means that you can use it for any purpose ___without any warranty___. :warning:

# Architecture

This is a single class application, which has a method that returns the new String, or throws an exception `IllegalArgumentException` in case the length requested is not greater than 0.

* **StringGenerator**: generates a new String;

The application will iterate over an array of letters and numbers and will return the amount of requested characters.

# Code

I'm not attaching here the Main.class, which is only an class to exemplify with the execution of the application. If necessary, visit my <i class="fa fa-github"></i> [GitHub][github] page.

#### StringGenerator.class
{{< gist rwanderc 28d1b684e4298e7ed1451c23a8eca592 >}}
