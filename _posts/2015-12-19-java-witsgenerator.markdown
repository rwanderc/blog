---
layout:      post
title:       "WitsGenerator"
subtitle:    "an application to generate random Wits data"
description: "A Java command-line application to generate random Wits data."
date:        2015-12-19 04:43:00
author:      "Wander Costa"
header-img:  "img/post-bg-rig.jpg"
thumb-img:   "img/thumb/thumb-java-witsgenerator.jpg"
---

[github]:https://github.com/rwanderc
[code]:https://github.com/rwanderc/WitsGenerator

Carrying on with the context of Oil & Gas, in this post I will share a very simple Java application to stream random Wits data, provided through a TCP Socket Server to a single client.

You will need Java 8 to build the application, or some previous version if you take out the diamond references and maybe some other stuff. No additional libraries are being used.

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

#Code

I'm not attaching here the Main.class, which is only an interface with the application. If necessary, visit my <i class="fa fa-github"></i> [GitHub][github] page.

#### WitsServer.class

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
package com.wandercosta.witsgenerator;

import com.wandercosta.witsgenerator.generator.WitsGenerator;
import com.wandercosta.witsgenerator.connection.TcpServer;

/**
 * This class is responsible for starting the TCP Server thread and writing to
 * its clients from time to time, according to the frequency configured.
 *
 * @author Wander Costa (www.wandercosta.com)
 */
public class WitsServer {

    private final TcpServer server;
    private final WitsGenerator generator;
    private final int frequency;

    public WitsServer(int port, int frequency, int records, int items) {

        this.server = new TcpServer(port);
        this.generator = new WitsGenerator(records, items);
        this.frequency = frequency;

    }

    public void start() {

        server.start();

        long spentTime, wait;

        while (true) {

            spentTime = System.currentTimeMillis();
            server.write(generator.generate());
            spentTime = -spentTime + System.currentTimeMillis();

            wait = frequency * 1000 - spentTime;

            try {
                Thread.sleep(wait);
            } catch (Throwable ex) {
            }

        }

    }

}
{% endhighlight %}

####TcpServer.class

{% highlight java%}
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
package com.wandercosta.witsgenerator.connection;

import java.io.PrintStream;
import java.net.ServerSocket;
import java.net.Socket;

/**
 * This is the TCP Socket Server that will open the port for the client to get
 * connected. It only accepts one client connected.
 *
 * @author Wander Costa (www.wandercosta.com)
 */
public class TcpServer extends Thread {

    private final int port;
    private ServerSocket socket;
    private Socket client;
    private PrintStream clientOut;

    public TcpServer(int port) {
        this.port = port;
    }

    @Override
    public void run() {

        try {

            socket = new ServerSocket(port);

            client = socket.accept();
            clientOut = new PrintStream(client.getOutputStream());

        } catch (Exception ex) {
        }

    }

    public void write(String data) {

        if (client != null && clientOut != null && !client.isClosed()) {

            clientOut.print(data);

        }

    }

}
{% endhighlight %}

#### WitsGenerator.class

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
package com.wandercosta.witsgenerator.generator;

/**
 * This class is responsible for generating the Wits blocks.
 *
 * @author Wander Costa (www.wandercosta.com)
 */
public class WitsGenerator {

    private final WitsLineGenerator lineGenerator;
    private final int records;
    private final int items;

    public WitsGenerator(int records, int items) {

        this.lineGenerator = new WitsLineGenerator();
        this.records = records;
        this.items = items;

    }

    public String generate() {

        StringBuilder str = new StringBuilder();

        for (int currentRecord = 1; currentRecord <= records; currentRecord++) {

            str.append("&&\n");

            for (int currentItem = 1; currentItem <= items; currentItem++) {

                str.append(lineGenerator.generate(currentRecord, currentItem)).append("\n");

            }

            str.append("!!\n");

        }

        return str.toString();

    }

}
{% endhighlight %}

#### WitsLineGenerator.class

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
package com.wandercosta.witsgenerator.generator;

import java.util.Random;

/**
 * Responsible for creating each data line. It uses a random interval between
 * -10000 and 10000 to generate the data, and does not guarantee any trend.
 *
 * @author Wander Costa (www.wandercosta.com)
 */
public class WitsLineGenerator {

    private static final float MAX_VALUE = 10000;
    private static final float MIN_VALUE = -10000;
    private static final Random r = new Random();

    private float randomValue() {

        return (r.nextFloat() * (MAX_VALUE - MIN_VALUE)) + MIN_VALUE;

    }

    public String generate(int record, int item) {

        String recordStr = String.valueOf(record);
        String itemStr = String.valueOf(item);
        String valueStr = String.valueOf(randomValue());

        if (recordStr.length() == 1) {
            recordStr = "0".concat(recordStr);
        }
        if (itemStr.length() == 1) {
            itemStr = "0".concat(itemStr);
        }

        return recordStr.concat(itemStr).concat(valueStr);

    }

}
{% endhighlight %}
