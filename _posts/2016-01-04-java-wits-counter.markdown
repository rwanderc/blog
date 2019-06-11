---
shorturl:    "http://goo.gl/D9Amn4"
layout:      post
title:       "WitsCounter"
subtitle:    "counting Wits parameters throughput in real time"
description: "A Java application to count Wits parameters throughput in real time."
date:        2016-01-04 20:04:00
author:      "Wander Costa"
header-img:  "/img/post-bg-rig2.jpg"
thumb-img:   "/img/post-thumb-rig2.jpg"
img-credits: "Winter"
img-credits-url: https://www.freepik.com/free-photos-vectors/winter
tags:
- java
- wits
- oil
- O&G
- witsml
- drilling
---

[github]:https://github.com/rwanderc
[git-witscounter]:https://github.com/rwanderc/wits-counter
[blog-witsgenerator]:../java-witsgenerator

Recently, I published a post of an application called [WitsGenerator][blog-witsgenerator] to provide random Wits data through a TCP port. Today I'm publishing another application related to the Oil & Gas business, but now it's to count the amount of parameters being transmitted through Wits.<!--more-->

For this project, I'm using:

* Java JDK 8u73;
* Maven 3.3.9.

This [code][git-witscounter] is also available in my <i class="fa fa-github"></i> [GitHub][github] page. Feel free to comment if you have any doubt or suggestion. :smile:

### Licensing
This software is licensed under the MIT license, what basically means that you can use it for any purpose ___without any warranty___. :warning:

# Functionality
This simple application provides 2 different information:

* The total amount of parameters per time interval;
* The amount of different parameters per time interval.

![post-img-java-witscounter-printscreen1.jpg](/img/post-img-java-witscounter-printscreen1.jpg)


# Architecture
The application works as an implementation of Producer-Consumer over a matrix data structure. This matrix works as a memory and each cell of this matrix represents, in any given moment, the amount of record-item received for that position since last matrix clean up. If the number 3 is found in the position `0x0`, it means that the `record 1 item 1` was received 3 times since the last matrix clean up. If the number 2 is found in the position `9x3`, it means that the `record 10 item 4` was received 2 times since the last matrix clean up.
To count the total amount of items received in a given moment, it's necessary to do a big sum of all the values present in the matrix. To count the number of unique items received in a given moment, it's necessary to do a sum of the number of cells which values differs to 0.
This matrix is based in the type `byte`, that offers good sizing for big amount of time, and, at the same time, is cheaper in terms of memory.

The Producer-Consumer aspect comes from the way the matrix is used: one thread keeps reading data from the Wits server and adds into the matrix, while another thread, from time to time, counts the total and unique values in the matrix and cleans it up.

Thus, there are 5 important classes:

* **WitsCounter.java**: is the class that orchestrates the start/stop of the resources;
* **WitsMatrixCounter.java**: is the implementation of the matrix used to store current counters;
* **WitsCountConsumer.java**: is the class responsible for consuming the data from the matrix and printing into the console;
* **WitsCountProducer.java**: is the class responsible for reading the data from the socket and adding to the matrix;
* **TCPClient.java**: is the representation of the TCP connection to the Wits server.

The usage is as follow:

{% highlight text %}
Usage: java -jar wits-counter-1.1.jar [host] [port] [interval]
Connects to a Wits server through TCP Socket and prints the number
of parameters being received per time interval. It does not
reconnect if the connection fails.
  [host]	      The IP of hostname of the server
  [port]	      The port of the server
  [interval]	  The time interval, in milliseconds, to aggregate
		            values. If less or equal 0, 1000 is used.
{% endhighlight %}

When executed, it will start a TCP Client socket client to the data provider, and will start counting the parameters being received.
No major treatment is done in the inputs, so you may have errors when wrong/unreal parameters are passed.

# Code
I'm not attaching here the Main.class. If necessary, visit my <i class="fa fa-github"></i> [GitHub][github] page.

#### WitsCounter.java
<script src="https://gist.github.com/rwanderc/117f0ce00d60832d9d50e2043284f881.js"></script>

#### WitsMatrixCounter.java
<script src="https://gist.github.com/rwanderc/2440e3e83c16c5d2b201139b2caaf2b8.js"></script>

#### WitsCountConsumer.java
<script src="https://gist.github.com/rwanderc/4cbd7dedaca99f0257e131c5a3b3f7f2.js"></script>

#### WitsCountProducer.java
<script src="https://gist.github.com/rwanderc/7a7bb59971d89cc18f8811b4f2bcbd8b.js"></script>

#### TCPClient.java
<script src="https://gist.github.com/rwanderc/5c5e4dc98d0ff46d1d809c85dbeeb92b.js"></script>
