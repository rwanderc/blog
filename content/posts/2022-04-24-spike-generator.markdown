---
layout:      "post"
url:         "/spike-generator/"
title:       "Spike detection"
subtitle:    "Simulation and detection of spikes in data streams"
date:        "2022-04-24 09:09:00"
author:      "Wander Costa"
tags:

- software development
- engineering management
- spikes

---

If you are a "show me code" dude, jump directly to the [repository][repository].

The year of 2014 was an interesting one to me. I joined a company and worked in a project for a couple of months in
Kuala Lumpur, Malaysia. It was again in the same industry I already worked for years already, but with a different set
of experiences. I remembered I wanted to put some thoughts into a problem I faced years back and happened to face again
there, in Malaysia: **detection of spikes in real time data streams**. Then I drafted an algorithm to detect those
spikes, but never really wrote it down, until this post.

At that moment, I was trying to find out a solution to a problem in the Oil & Gas industry, but eventually I found out
it to be an [interesting problem also in neuroscience][neuroscience] as well. In this post, I cover the original
problem, but given the same aspects, the solution can be replicated to other scenarios.

# The Oil & Gas telemetry problem

In the Oil & Gas industry, there are multiple types of operations until a reservoir can be explored, extracting crude
oil or gas from it. Depending on the reservoir, multiple wells are drilled in a certain way to maximize its production
afterward. As you can imagine, it's a complex and expensive operation and has many phases.

One of them is the **drilling** phase. There are huge complexities in this operation, not only regarding the concern on
security, but due to many other factors like the length of the well (sometimes 5 kilometers deep; and pre-salt even
deeper), the depth of the water layer (for offshore wells), the high pressure that needs to be controlled to avoid
influx or a potential fracture of the well, the necessity for quick feedback from geology conditions to mitigate risks
as well as to land the well in a position to maximize its posterior production, etc.

Among many details of this operation, there is also a special mud prepared by chemical engineers, injected into the
drilling column, from rig until the bit, in order to control pressure and lift sediments up to the rig while the bit is
cutting them. In operations with MWD (measurement while drilling) tools, therefore with the need for real-time
monitoring of the geological responses downhole, near the bit, the communication with these tools is done without wires.
It's actually based on [telemetry][telemetry] using the mud as transport layer. The MWD tools transmit information by
pumping the mud downhole in a frequency that the computers at the rig can read, decode and convert into data.

If you want to know more about the Well Drilling and Well Construction Process, there are some
good [videos in Youtube][wellconstructionvideo] explaining with many more details.

But where do the spikes appear? In these circumstances, **telemetry has a lot of interference**, which affects the
reading of the pulses and causes some outliers in the data. These spikes or valleys are not real measurements. Instead,
they are errors in the reading of the pulses and usually require manual identification and removal.

![](https://www.researchgate.net/publication/257709973/figure/fig1/AS:866939387781120@1583705865271/Typical-example-of-noise-in-the-steerable-drilling-downward-command-signal.png)

COLOCAR OUTRA IMAGEM EXEMPLIFICANDO MELHOR!

# Mathematical background

Detecting spikes or valleys in discrete series of data is a matter of identifying the discontinuity of the curve,
associated with some other indications. But if it's a discrete series of data, there is no concept of continuity or
discontinuity. Then we need to understand a couple of ideas and work on their analog applications for discrete series.

### Speed

The speed of a curve is given by its first derivative, and can ultimately be used to calculate the angle of the curve in
a specific point. In the parabola below, described by the expression `f(x) = x^2`, its first derivative is expressed
by `f'(x) = 2x`. And from that, it's possible to calculate the exact angle of the curve in any point by the
expression `g(x) = atan(f'(x))`, in radians, or `h(x) = atan(f'(x)) * 180/π`, in degrees.

![](/img/spikes-xx-2x-angle.png)

Looking to the graph of `f(x)`, it's easy to find out where the curve changed its direction. And this is also reflected
in the graph for `f'(x)`, since the speed was `0`, coming from negative to positive.

It's also interesting to observe the relation between the `f(x)` and `g(x)`: when `x` tends to `-∞`, `g(x)` tends
to `-90`; and when `x` tends to `+∞`, `g(x)` tends to `90`, however, it never touches `-90` or `90`. And it's intuitive
to read this from `f(x)`, since having either `-90` or `90` would mean `f(x)` would have multiple values for the
same `x`, which is impossible.

For this work, it's important to understand the relation of the first derivative and the angle and the curve, but for
this work, we won't need to calculate angles in radians or degrees. Therefore, we can focus on the derivatives only.

### Acceleration

The acceleration of a curve is an extension of the concept of the speed, and is given by the second derivative.
Therefore, for the function `f(x) = x^2`, the first derivative is given by the expression `f'(x) = 2x` while the second
derivative is given by the expression `f''(x) = 2`, which is a constant. In other words, `f(x)` has constant
acceleration.

![](/img/spikes-xx-2x-2.png)

An important aspect of the acceleration that will be useful when writing the algorithm is that the acceleration is
positive, which means that the curvature points upwards. If `f(x) = -x^2`, it's acceleration would be given
by `f''(x) = -2` instead, and its curvature would point downward.

![](/img/spikes-xx-2x-2-negative.png)

For both examples above, however, the acceleration was constant. Let's analyse another function, where we have a
different behaviour: `f(x) = x^3 - 15x`, `f'(x) = 3x^2 - 15` and `f''(x) = 6x`. From the graphs, it's easy to spot the 2
moments where the curve changes the direction, i.e. the speed reached `f'(x) = 0`: `x = sqrt(5)` and `x = -sqrt(5)`.

![](/img/spikes-xxx-3x2-6x.png)

The graph of the acceleration `f''(x)` does not provide information about the change of the direction. However, it
provides different and important insight: the indication of the change of the curvature of the parabola. Looking at the
graph from `f''(x)`, it's easy to find where `f(x)` stopped behaving as a parabola pointing downward and started
behaving as a parabola pointing upward, since the acceleration reached `0`, coming from negative to positive.

> The graph of a function with a positive second derivative is upwardly concave, while the graph of a function with a
> negative second derivative curves in the opposite way.

https://en.wikipedia.org/wiki/Second_derivative

The understanding of these insights is relevant for the understanding of the algorithm:

- when the curve changes its direction
- when the curve changes its curvature

### Speed and Acceleration in discrete series

Discrete series of data are by nature not continuous. Therefore instead of calculating the tangent in a specific point,
the straight line between two points is used. Speed and acceleration have their analog meanings for discrete series
considering this logic.

![](/img/spikes-xx-discrete.png)

![](/img/spikes-xxx-discrete.png)

The speed of discrete series of data can be calculated as the angle between two points, i.e. the quotient of the
differences between `(x0, y0)` and `(x1, y1)`: `dY/dX = (y1-y0)/(x1-x0)`.

For a series of data points `S1(x,y) = {[1,1], [2,2], [3,3]}`, the speed between 1st and 2nd points, and between 2nd and
3rd points equals `1` (`dY/dX = (2-1)/(2-1)` and `dY/dX = (3-2)/(3-2)`). However, for the series of data
points `S2(x,y) = {[1,1], [2,2], [3,10]}`, the speed between 1st and 2nd points is `1` (`dY/dX = (2-1)/2-1)`) while
between 2nd and 3rd is `8` (`dY/dX = (10-2)/(3-2)`). Then the series of speeds are
respectively `S1'(x,y) = {[1,1], [2,1]}` and `S2'(x,y) = {[1,1], [2,8]}`

![](/img/spikes-discrete-examples.png)

Like the 2nd derivative, the acceleration of the discrete series can be calculated as an extension of the speed, i.e.
the quotient of the differences between two speeds `(x'0, y'0)` and `(x'1, y'1)`: `d'Y/dX = (y'1-y'0)/(x'1-x'0)`.
Therefore, given the previous data sets, the acceleration will be given by `S1''(x,y) = {[1,0]}`
and `S2''(x,y) = {[1, 7]}`. That is, the acceleration of S1 is `0` while the acceleration of S2 is `7` for the sets
specified.

# Identifying Spikes

When identifying spikes or valleys in data sets, it's important to keep the focus on the acceleration of the data set.
And the reason for this is that the acceleration will easily show the changes in the tendency of the data set.

However, how much acceleration change is considered a potential spike or valley spot?

The answer depends on the actual behaviour and tendency of dispersion of the data set. Therefore, it's also important
measure the **Standard Deviation** of the acceleration to define what's acceptable and what's not.

# Spike simulator

![](/img/spikes-simulator-sampling.png)

# Contributions

Did you find any wrong information? Please help me fix it commenting below. I'd be happy to receive reviews and
contribution to this article.

[neuroscience]: https://www.frontiersin.org/articles/10.3389/fninf.2015.00028/full

[repository]: https://github.com/rwanderc/spikes

[telemetry]: https://glossary.oilfield.slb.com/en/terms/t/telemetry

[wellconstructionvideo]: https://www.youtube.com/watch?v=HHip4mkTrQs

---

https://www.google.com/search?q=derivative+angle+curve+discrete+series&ei=dVdoYqzIGu_-7_UP596u6Ac&ved=0ahUKEwjs1o6EyrL3AhVv_7sIHWevC30Q4dUDCA4&uact=5&oq=derivative+angle+curve+discrete+series&gs_lcp=Cgdnd3Mtd2l6EAMyBQghEKABOgcIABBHELADOgcIIRAKEKABSgQIQRgASgQIRhgAUN8EWI4JYOMLaAFwAXgBgAGGAYgBnwWSAQM2LjGYAQCgAQHIAQfAAQE&sclient=gws-wiz

https://page.math.tu-berlin.de/~techter/lecture_notes/ss14-geometry2-ddg.pdf