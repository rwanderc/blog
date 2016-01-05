---
layout:      post
title:       "WitsCounter"
subtitle:    "counting Wits parameters throughput in real time"
description: "A Java application to count Wits parameters throughput in real time."
date:        2016-01-04 20:04:00
author:      "Wander Costa"
header-img:  "/img/post-bg-rig.jpg"
thumb-img:   "/img/thumb/thumb-java-witscounter.jpg"
---

[github]:https://github.com/rwanderc
[git-witscounter]:https://github.com/rwanderc/WitsCounter
[blog-witsgenerator]:./java-witsgenerator

Recently, I published a post of an application called [WitsGenerator][blog-witsgenerator] to provide random Wits data through a TCP port. Today I'm publishing another application related to the Oil & Gas business, but now it's to count the amount of parameters being transmitted through Wits.

This application was compiled using Java 8, but you can compile also in previous versions of Java (maybe some adjustments will be necessary).

This [code][git-witscounter] is also available in my <i class="fa fa-github"></i> [GitHub][github] page. Feel free to comment if you have any doubt or suggestion. :smile:

### Licensing

This software is licensed under the MIT license, what basically means that you can use it for any purpose ___without any warranty___. :warning:

# Functionality

This simple application provides 2 different information:

* The total amount of parameters per time interval;
* The amount of different parameters per time interval.

![post-img-java-witscounter-printscreen1.jpg](/img/post-img-java-witscounter-printscreen1.jpg)


# Architecture

The application is based in two classes:

* **Main**: provides a command-line interface;
* **WitsCounter**: creates the Wits client and processes the count from time to time.

The usage is as follow:

{% highlight text %}
Usage: java -jar WitsCounter.jar [host] [port] [interval]
Connects to a Wits server through TCP Socket and prints the number
of parameters being received per time interval. If Does not reconnect if connection fails.

Mandatory arguments
  host          The IP of hostname of the server
  port          The port of the server
  interval      The time interval, in milliseconds, to aggregate
                values. If less or equal to 0, 1000 is used.
{% endhighlight %}

When executed, it will start a TCP Client socket to the data provider, and will start counting the parameters being received. It uses an bidimensional array to represent the individual parameters received in the time frame, so as to provide the individual count.

No treatment is done in the inputs, so you may have errors when wrong/unreal parameters are passed.

#Code

I'm not attaching here the Main.class, which is only an interface with the application. If necessary, visit my <i class="fa fa-github"></i> [GitHub][github] page.

#### WitsCounter.class

{% highlight java %}
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
package com.wandercosta.witscounter;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.Socket;

/**
 * A Wits client that connects to a Wits server through TCP and counts the
 * number of parameters (total and individual) being received per time interval.
 *
 * @author Wander Costa (www.wandercosta.com)
 */
public class WitsCounter {

    private final Object counterSemaphore;
    private int totalCounter;
    private final boolean[][] usedMapping;

    public WitsCounter() {

        this.counterSemaphore = new Object();
        this.totalCounter = 0;
        this.usedMapping = new boolean[99][99];

    }

    public void start(String host, Integer port, Integer interval) {

        if (interval <= 0) {
            interval = 1000;
            System.out.println("Overwriting interval to 1000ms...");
        }

        try {

            startClient(host, port);

            sleepUntilNextInterval(1000);

            int countTotalParameters;

            while (true) {

                synchronized (counterSemaphore) {
                    countTotalParameters = totalCounter;
                    totalCounter = 0;
                }

                System.out.println(countTotalParameters + " total var(s) / " + interval + " ms");
                System.out.println(countIndividualParameters(usedMapping) + " diff var(s) / " + interval + " ms");

                sleepUntilNextInterval(interval);

            }

        } catch (Throwable ex) {

            System.err.println("Unknown error. Halting...");
            System.exit(1);

        }

    }

    private void startClient(String host, int port) throws IOException {

        Socket client = new Socket(host, port);
        BufferedReader reader = new BufferedReader(new InputStreamReader(client.getInputStream()));

        new Thread() {

            @Override
            public void run() {

                String text = null;

                while (true) {

                    try {
                        text = reader.readLine();
                    } catch (Throwable ex) {
                    }

                    if (text != null) {
                        countTotal(text);
                    }

                }

            }

        }.start();
    }

    private void countTotal(String data) {

        if (data.isEmpty()) {
            return;
        }

        int tmpCounter = 0;

        do {

            int linebreak = data.indexOf("\n");
            String line;

            if (linebreak == -1) {
                line = data;
                data = "";
            } else {
                line = data.substring(0, linebreak);
                data = data.substring(linebreak + 1, data.length());
            }

            if (line != null) {

                if (!line.startsWith("&&") && !line.startsWith("!!")) {

                    try {

                        int recIndex = Integer.valueOf(line.substring(0, 2));
                        int itemIndex = Integer.valueOf(line.substring(2, 4));

                        usedMapping[recIndex - 1][itemIndex - 1] = true;
                        tmpCounter++;

                    } catch (Throwable ex) {

                    }

                }

            }

        } while (!data.isEmpty());

        synchronized (counterSemaphore) {
            totalCounter += tmpCounter;
        }

    }

    private int countIndividualParameters(boolean[][] mapping) {

        int individualParameters = 0;

        for (boolean[] record : mapping) {
            for (boolean item : record) {
                if (item) {
                    individualParameters++;
                    item = false;
                }
            }
        }

        return individualParameters;

    }

    private void sleepUntilNextInterval(long interval) {

        long sleep = interval - (System.currentTimeMillis() % interval);
        try {
            Thread.sleep(sleep);
        } catch (Throwable ex) {
        }

    }

}
{% endhighlight %}
