---
layout:      "post"
url:         "/spike-generator-background/"
title:       "Spike detection background"
subtitle:    "Simulation and detection of spikes in real time data streams"
date:        "2022-04-24 09:08:00"
author:      "Wander Costa"
tags:

- software development
- engineering management
- spikes

---

If you are a "show me code" person, jump directly to the [repository][repository]. Otherwise, enjoy the explanation of
the algorithm.

# The Oil & Gas telemetry problem

In 2014, I joined a company and worked in a project for a couple of months in Kuala Lumpur, Malaysia. It was again in
the same industry I already worked for years already, but now with a different set of experiences. I remembered I wanted
to put some thoughts into a problem I faced years before and happened to face again there, in Malaysia: **detection of
spikes in real time data streams**. I drafted an algorithm to detect those spikes.

At that moment, I was trying to find out a solution to a problem in the Oil & Gas industry, but eventually I found out
it to be an [actual problem in neuroscience][neuroscience] as well. In this post, I cover the Oil & Gas problem, but
given the same aspects, the solution can be replicated to other scenarios.

In the Oil & Gas industry, there are multiple types of operations executed until a reservoir can be explored, extracting
crude oil or gas from it. Depending on the reservoir, multiple wells are drilled in a certain way to maximize their
production afterward. As you can imagine, it’s a complex and expensive operation and has many phases.

One of those phases is called **drilling**. There are many complexities in this operation, not only regarding the
concern on security but also due to many other factors like the length of the well (sometimes 5 kilometers deep; and
even deeper for pre-salt exploration), the depth of the water layer (for offshore wells), the high pressure that needs
to be controlled to avoid a potential fracture of the well and influx, the necessity for quick feedback from geology
conditions to mitigate risks as well as to land the well in a position to maximize its posterior production, etc.

Among many other details of this operation, there is a special mud prepared by chemical engineers, injected into the
drilling column, from the rig until the bit, to control pressure and lift sediments to the rig while the bit is cutting
them. In operations with MWD (measurement while drilling) tools, therefore with the need for real-time monitoring of the
geological responses downhole, near the bit, the communication with these tools is done without wires. It’s based on
[telemetry][telemetry] using the mud as a transport layer. The MWD tools transmit information by pumping the mud
downhole at a frequency that the computers at the rig can read, decode and convert into data.

If you want to know more about the Well Drilling and Well Construction Process, there are some
good [videos in Youtube][wellconstructionvideo] explaining many more details.

**But where do the spikes appear?** In these circumstances, **telemetry has a lot of interference**, which affects the
reading of the pulses and causes some outliers in the data. These spikes or valleys are not real measurements. Instead,
they are errors in the reading of the pulses and usually require manual identification and removal.

# Mathematical background

Detecting spikes or valleys in discrete series of data is a matter of identifying the discontinuity of the curve,
associated with some other indications. But if it's a discrete series of data, there is no concept of continuity or
discontinuity. Then it's important to understand a couple of ideas and work on their analog applications for discrete
series.

### Speed

The speed of a curve is given by its first derivative, and can ultimately be used to calculate the angle of the curve in
a specific point, i.e. the tangent in a specific point. In the parabola below, described by the expression `f(x) = x^2`,
its first derivative is expressed by `f'(x) = 2x`. And from that, it's possible to calculate the exact angle of the
curve in any point by the expression `g(x) = atan(f'(x))`, in radians, or `h(x) = atan(f'(x)) * 180/π`, in degrees.

![](/img/spikes-xx-2x-angle.png)

Looking to the graph of `f(x)`, it's easy to find out where the curve changed its direction. And this is also reflected
in the graph for `f'(x)`, since the speed transits from negative to positive passing through `0`.

It's also interesting to observe the relation between the `f(x)` and `g(x)`: when `x` tends to `-∞`, `g(x)` tends
to `-90`; and when `x` tends to `+∞`, `g(x)` tends to `90`, however, it never touches `-90` or `90`. And it's intuitive
to read this from `f(x)`, since having either `-90` or `90` would mean `f(x)` would have multiple values for the
same `x`, which is impossible.

For this work, it's interesting to understand the relation of the first derivative, the angle and the curve, but it
won't be necessary to calculate angles in radians or degrees. Therefore, keep focusing on the derivatives only.

### Acceleration

The acceleration of a curve is an extension of the concept of the speed, and is given by the second derivative.
Therefore, for the function `f(x) = x^2`, the first derivative is given by the expression `f'(x) = 2x` while the second
derivative is given by the expression `f''(x) = 2`, which is a constant. In other words, `f(x)` has constant
acceleration for every value of `x`.

![](/img/spikes-xx-2x-2.png)

An important aspect of the acceleration is the signal. If the acceleration is positive, it means that the curvature
points upwards. If `f(x) = -x^2`, it's acceleration would be given by `f''(x) = -2` instead, and its curvature would
point downward.

![](/img/spikes-xx-2x-2-negative.png)

For both examples above, however, the acceleration is constant. Take as example another set of functions, with a
different behaviour: `f(x) = x^3 - 15x`, `f'(x) = 3x^2 - 15` and `f''(x) = 6x`. From the second graph, it's easy to spot
the 2 moments where the curve changes the direction, from positive to negative, and negative to positive again, i.e. the
speed reached `f'(x) = 0` for both `x = sqrt(5)` and `x = -sqrt(5)`.

![](/img/spikes-xxx-3x2-6x.png)

The graph of the acceleration `f''(x)` does not provide information about the change of the direction. However, it
provides different and important insight: the indication of the change of the curvature of the parabola. Looking at the
graph from `f''(x)`, it's easy to find where `f(x)` stopped behaving as a parabola pointing downward and started
behaving as a parabola pointing upward, since the acceleration reached `0`, coming from negative to positive.

> The graph of a function with a positive second derivative is upwardly concave, while the graph of a function with a
> negative second derivative curves in the opposite way. https://en.wikipedia.org/wiki/Second_derivative

For the scope of the algorithm, it's important to understand how to identify:

- When the curve changes its direction: 1st derivative changes signal
- When the curve changes its curvature: 2nd derivative changes signal

### Speed and Acceleration in discrete series

Discrete series of data are by nature not continuous. Therefore instead of calculating the tangent in a specific point,
the straight line between two points is used. Speed and acceleration have their analog meanings for discrete series
considering the same logic.

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

# Contributions

I want to thank Patrick Ponte for helping and reviewing the Oil & Gas content.

[neuroscience]: https://www.frontiersin.org/articles/10.3389/fninf.2015.00028/full

[repository]: https://github.com/rwanderc/spikes

[telemetry]: https://glossary.oilfield.slb.com/en/terms/t/telemetry

[wellconstructionvideo]: https://www.youtube.com/watch?v=HHip4mkTrQs