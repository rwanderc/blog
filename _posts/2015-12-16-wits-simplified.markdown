---
layout:      post
title:       "Wits Simplified"
subtitle:    "understanding data transmission in Oil & Gas operations"
description: "Understanding data transmission in Oil & Gas operations, from a developer's perspective"
date:        2015-12-16 18:12:00
author:      "Wander Costa"
header-img: "img/post-bg-rig.jpg"
---

Yeah, I know this is a very specific domain knowledge, and probably if you are not involved with the Oil & Gas industry, it's not gonna make any sense for you. However, it's interesting to know that Oil & Gas operations have a very large mass of data of all kind, transmitted in real time, to make it possible to monitor the well construction process and mainly to mitigate the risks of failures, that sometimes cause fatal accidents or huge damages to the environment ...

In this article, I will provide a little understanding of the **Wits** protocol. There are other transmission protocols, like [WitsML][witsml] that is newer, more powerful in some ways, but much more complex and dependant of good infrastructure. WitsML might be subject of other posts in the future, but keep in mind that Wits and WitsML are totally different protocols for the same purpose.

#Wits

> The WELLSITE INFORMATION TRANSFER SPECIFICATION (WITS) is a communications format used for the transfer of a wide variety of wellsite data from one computer system to another. It is a recommended format by which Operating and Service companies involved in the Exploration and Production areas of the Petroleum Industry may exchange data in either an online or batch transfer mode.

In other terms, **[Wits][wits]** (Wellsite Information Transfer Specification) is a  specification used in the Oil & Gas industry to provide real time data transmission. Its structure is quite simple: **items** within **records** within **blocks**; transmission over raw **TCP socket** or **Serial**.

Simply, it works based on an aggreement between transmissor and receptor to consider **record** X **item** Y to be an specific data in a specific **unit**.

Blocks are delimited by the `&&` and `!!` lines and always refer to a single record. Records are identified by a numeric string containing the 1st and 2nd characters of the data line; items are identified by the numeric string containing the 3rd and 4th characters; and the data itself is identified by the rest of the line.

{% highlight text %}
&&
01082319.024
01103152.102
011221.31
!!
{% endhighlight %}

Considering the example above, we have one block, with 3 data lines, in which the line `01082319.024` refers to:

*   Record: `01`
*   Item: `08`
*   Value: `2319.024`

Both sides of the transmission need to be aligned about what record 01 item 08 means. Suppose, for example, it's the Bit Depth (measured) in meters. If both sides are aware about it, transmission will be just fine. However, if there is misunderstanding of one side, expecting Bit Depth in feet, the data will make no sense any longer.

All the mapping needs to be known and well aligned between both sides, so as to prevent these kind of mistakes. And that's what turns Wits to be a little weak in terms of consistency, but in other hands very efficient in terms of flexibility.

###Consistency and Flexibility

**Time is money!** The Oil & Gas operations are still very expensive, and operators push the service companies to be very optimized... One of the results of this pressure is seem in the lack of time and effort to make data transmission to be correct from the beginning. And, of course, **first things first**. Making sure the rig doesn't have a risky scenario is uncomparable more important than making transmission aligned.

In the end, it causes the transmission to be the last concern of the companies at the rigs, and this is where the consistency not enforced by the protocol appears.

However, being simple as 1 2 3 makes configuration of Wits a simple setup, as well as makes it easier to create new software to transmit it. Having this fact, associated to the protocol's maturity, the compatibility of the protocol with the industry is huge.

##Versions

There are some different versions of Wits but as per my experience the most widely used are Level 0 and Level 1. A big difference between these two is that Level 1 has twenty five predefined records and items identified, covering drilling, geology, directional work, MWD, cementing and testing. Having these pre-defined records makes a little easier to maintaing an aggreement between the sides.

## Sources

[WITS - Wellsite Information Transfer Specification][wits]

[WITSML - Current Standards][witsml]


[wits]:http://home.sprynet.com/~carob/ 
[witsml]:http://www.energistics.org/drilling-completions-interventions/witsml-standards/current-standards
