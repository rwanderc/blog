---
layout:      post
title:       "Your first SpringBoot app in 5 steps"
subtitle:    "An example of SpringBoot you can create in 5 steps"
description: "An example of SpringBoot you can create in 5 steps"
date:        2019-09-21 20:18:00
author:      "Wander Costa"
header-img:  "/img/post-bg-boots.jpg"
thumb-img:   "/img/post-thumb-boots.jpg"
img-credits: "jcomp"
img-credits-url: https://www.freepik.com/free-photos-vectors/travel
img-credits-real-url: https://www.freepik.com/free-photo/hiking-female-boots_3709511.htm#page=8&query=boot&position=38
tags:
- java
- clean code
- spring boot
- spring
- rest
- microservices
- software engineering
- software development
- development
---

**SpringBoot** is made to be powerful and easy! It's very nice to work with and you will need only these 5 steps for your first app in a Maven project:

1. Define the SpringBoot parent lib
2. Define the dependency to the SpringBoot web library
3. Define the Maven plugin for SpringBoot
4. Create the main method
5. Create a controller with an endpoint

## Parent SpringBoot lib

This is the main and first step when creating a SpringBoot application. The parent library will import basic dependencies from Spring into your app. Add the following code into your pom.xml.

<script src="https://gist.github.com/rwanderc/638eb07e2b5812260f6c025d1a6a8b2b.js"></script>

## Web Starter

The second step is to import the web started dependency. This dependency will trigger the initialization of the web container (Tomcat, by default) and will make listen to HTTP requests.

<script src="https://gist.github.com/rwanderc/61f719bc121e05e1254a59eb448437ef.js"></script>

## SpringBoot Plugin

In order to easily make the JAR file call the main class when executed, you need the spring-boot-maven-plugin. Add this to the pom.xml.

<script src="https://gist.github.com/rwanderc/10d098ffd4fe1babfd942e8d5b48a363.js"></script>

In the end, your pom.xml file will look like:

<script src="https://gist.github.com/rwanderc/c012975e5f38dc3b2ef5e1c1d91433d6.js"></script>

## Main Method

As any other executable Java application, it will need a class with a main method so as to start the application. For SpringBoot applications, your main method must call SpringBoot's **run()** method, as below.

<script src="https://gist.github.com/rwanderc/5310f0e431e88a7a83389ef3c904be2a.js"></script>

## Controller

Now, after setting up SpringBoot, all you need is an endpoint to provide data. In the example below, the endpoint is implemented in the same class as the main class, but this is not a requirement. It could have been a different class.

<script src="https://gist.github.com/rwanderc/4e87d371caa364a1d12cadcb4f8ef76e.js"></script>

## Running the App

Before running the application, it must be compiled with the command **mvn install**. After compiled, the application can be run like any other Java JAR file. Run the command **java -jar spring-boot-first-app-1.0-SNAPSHOT.jar** in the target folder.

![img/spring-boot-startup-screen.png](/img/spring-boot-startup-screen.png)

And now the application can be accessed via the browser or via the curl command **curl http://localhost:8080**.

---

The code of this example is available in my [GitHub account][github].<br>
[Check it out][code-example].

[code-example]:https://github.com/rwanderc/examples/tree/master/spring-boot/spring-boot-first-app
[github]:https://github.com/rwanderc