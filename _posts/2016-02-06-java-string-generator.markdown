---
layout:      post
title:       "String Generator"
subtitle:    "how to generate strings programatically"
description: "How to generate Strings programatically."
date:        2016-02-06 18:00:00
author:      "Wander Costa"
header-img:  "/img/post-bg-programming.jpg"
thumb-img:   "/img/thumb/thumb-java-string-generator.jpg"
---

[github]:https://github.com/rwanderc
[git-stringgenerator]:https://github.com/rwanderc/StringGenerator

It's usually necessary, sometimes, to have a way to produce random strings programatically:

* Produce random IDs for entities;
* Produce random passwords;
* Produce random patterns, etc.

Today, I will show you a very quick class to produce those strings to use in your projects.
This was compiled using Java 8, but you can compile also in previous versions of Java (maybe some adjustments will be necessary).

This [code][git-stringgenerator] is also available in my <i class="fa fa-github"></i> [GitHub][github] page. Feel free to comment if you have any doubt or suggestion. :smile:

### Licensing

This software is licensed under the MIT license, what basically means that you can use it for any purpose ___without any warranty___. :warning:

# Architeture

This is a single class application, which has a method that returns the new String.

* **StringGenerator**: generates a new String;
* **Main**: is the executor.

The application will iterate over an array of letters and numbers and will return the amount of requested characters.

#Code

I'm not attaching here the Main.class, which is only an interface with the application. If necessary, visit my <i class="fa fa-github"></i> [GitHub][github] page.

#### StringGenerator.class

``` java
/**
 * The MIT License (MIT)
 *
 * Copyright (c) 2015 Wander Costa (www.wandercosta.com)
 *
 * Permission is hereby granted, free of charge, to any person obtaining a copy
 * of this software and associated documentation files (the "Software"), to deal
 * in the Software without restriction, including without limitation the rights
 * to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
 * copies of the Software, and to permit persons to whom the Software is
 * furnished to do so, subject to the following conditions:
 *
 * The above copyright notice and this permission notice shall be included in
 * all copies or substantial portions of the Software.
 *
 * THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
 * IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
 * FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
 * AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
 * LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
 * OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
 * SOFTWARE.
 */
package com.wandercosta.stringgenerator;

import java.util.Random;

/**
 * Simple class to generate random {@link String}s.
 *
 * @author Wander Costa (www.wandercosta.com)
 * @version 1.0
 */
public class StringGenerator {

    private static final String ABC = "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ";
    private final Random random;

    public StringGenerator() {

        this.random = new Random();

    }

    public String generate(int length) {

        StringBuilder sb = new StringBuilder(length);

        for (int i = 0; i < length; i++) {

            sb.append(ABC.charAt(random.nextInt(ABC.length())));

        }

        return sb.toString();

    }

}
```
