---
shorturl:    "http://goo.gl/fBxRii"
layout:      post
title:       "WitsGenerator"
subtitle:    "an application to generate random Wits data"
description: "A Java command-line application to generate random Wits data."
date:        2015-12-19 04:43:00
author:      "Wander Costa"
header-img:  "/img/post-bg-rig.jpg"
thumb-img:   "/img/thumb/thumb-java-witsgenerator.jpg"
tags:
- java
- wits
- witsml
- oil
- O&G
- drilling
---

[github]:https://github.com/rwanderc
[code]:https://github.com/rwanderc/WitsGenerator

Carrying on with the context of Oil & Gas, in this post I will share a very simple Java application to stream random Wits data, provided through a TCP Socket Server to a single client.

You will need Java 8 to build the application, or some previous version if you take out the diamond references and maybe some other stuff. Some additional libraries are being used for tests like JUnit and Mockito, as well as JaCoCo for code coverage.

This [code][code] is also available in my <i class="fa fa-github"></i> [GitHub][github] page. Feel free to comment if you have any doubt or suggestion. :smile:

### Licensing

This software is licensed under the MIT license, what basically means that you can use it for any purpose ___without any warranty___. :warning:

# Architecture

The application is based in 5 classes:

* **Main**: provides a command-line interface;
* **WitsServer**: coordinates the TCPServer thread and the WitsGenerator;
* **TCPServer**: a thread to stream the data into client's socket;
* **WitsGenerator**: generates the blocks of data, by assembling lines;
* **WitsLineGenerator**: generates each line of data.

The usage is as follow:

{% highlight text %}
Usage: java -jar WitsGenerator.jar [port] [frequency] [records] [items]
Creates a socket server and transmit Wits random data from time
to time. Only allows ONE single client connected.

Mandatory arguments
  port          The port to run the socket Server
  frequency     The frequency, in seconds, of the data generation
  records       The amount of records to be transmitted
  items         The amount of items to be transmitted in each reacord
{% endhighlight %}

When executed, it will start listening for the first client at ``port`` and produce ``records x items`` lines of data in each ``frequency`` seconds.

No treatment is done in the inputs, so you may have errors when wrong/unreal parameters are passed.

# Code

I'm not attaching here the Main.class, which is only an interface with the application. If necessary, visit my <i class="fa fa-github"></i> [GitHub][github] page.

#### WitsServer.class
<script src="https://gist.github.com/rwanderc/375a333b53e5201f879af7a8f69a47f3.js"></script>

#### TcpServer.class
<script src="https://gist.github.com/rwanderc/24309ba01c8292f39ce4100bd7fddc3d.js"></script>

#### WitsGenerator.class
<script src="https://gist.github.com/rwanderc/c2a67e2c1c326f659c2c8bacc6c32b96.js"></script>

#### WitsLineGenerator.class
<script src="https://gist.github.com/rwanderc/bbaa7811ae1071b75cf21b6ccee5917c.js"></script>
