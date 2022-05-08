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

In 2014, I drafted an algorithm to detect spikes in real-time data streams, but never really wrote it down. At that
moment, I was trying to find out a solution to a problem related to the Oil & Gas industry, but eventually I found out
it to be an [interesting problem also in neuroscience][neuroscience].

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
reading of the pulses and causes some outliers in the data. These spikes or valleys are not real measurements, are
errors in the reading of the pulses and are usually manually identified and removed.

![](https://www.researchgate.net/publication/257709973/figure/fig1/AS:866939387781120@1583705865271/Typical-example-of-noise-in-the-steerable-drilling-downward-command-signal.png)

COLOCAR OUTRA IMAGEM EXEMPLIFICANDO MELHOR!

# The solution

Detecting spikes or valleys in discrete series of data is a matter of identifying the discontinuity of the curve. But if
it's a discrete series of data, there is no concept of continuity or discontinuity. Then we need to understand a couple
of ideas and work on their applications for discrete series.

The speed of a curve is the same as the angle of the curve in a specific point. Therefore, in discrete series, the speed
is the quotient of the deltas: `dY / dX` = `y1-y0 / x1-x0`.

For a series of data points (x, y) `[1,1], [2,2], [3,10]`, the angle between 1st and 2nd points
equals `1` (`dY/dX = (2-1)/(2-1)`), while the angle between 2nd and 3rd points equals `8` (`dY/dX = (10-2)/(3-2)`).
However, for the series of data points (x, y) `[1,1], [2,2], [3,3]`, both angles between 1st and 2nd, and between 2nd
and 3rd equal `1` (`dY/dX = (2-1)/(2-1)` and `dY/dX = (3-2)/(3-2)` respectively). The differences between these 2 series
of data points is that the first one tends to accelerate while the second keeps a constant speed of growth.

EXCEL GRAPH

In other terms, the speed of a curve is how fast `y = f(x)` varies, that is `y' = f'(x)`. Considering the two data
series above:

- (x, y) `[1,1], [2,2], [3,10]` would generate the speed series (x, y') `[1,1], [2,8]`
- (x, y) `[1,1], [2,2], [3,3]` would generate the speed series (x, y') `[1,1], [2,1]`

And the acceleration of a curve is an extension of this concept. In other words, it's how fast the speed (not the `y`)
increases or decreases. That is `y'' = f''(x)`. Considering the two data series above:

- (x, y) `[1,1], [2,2], [3,10]` would generate the acceleration point (x, y'') `[1,7]`
- (x, y) `[1,1], [2,2], [3,3]` would generate the acceleration point (x, y'') `[1,0]`

That's exactly the concept of first and second derivatives of a curve (speed and acceleration) but applied to discrete
series of data. And that's the base of the algorithm.

### Acceleration

The acceleration of a curve is relevant because it's more sensitive to the variations of the tendency of the curve than
the speed itself, and because it's also an indication of the direction of the curve.

> The graph of a function with a positive second derivative is upwardly concave, while the graph of a function with a
> negative second derivative curves in the opposite way.

https://en.wikipedia.org/wiki/Second_derivative

The series of data points (x,y) `[1,1], [2,3], [3,5], [4,6], [5,6.5]` would result in all positive speeds, but some
negative acceleration points: (x,y') `[1,2], [2,2], [3,1], [4,0.5]` and (x,y'') `[1,0], [2,-1], [3,-0.5]`. It means that
the curve is changing the direction. However, the speed doesn't provide such information until `y` starts to the
decrease.

EXCEL GRAPH

### Identifying spikes

To identify spikes, we will compare the acceleration with a subset of other accelerations previously collected and
calculate if the new collected acceleration respects a certain threshold. The **Standard Deviation** expresses the level
of dispersion in a set of points and is a good starting point to base the threshold upon.

# Spike simulator

# Contributions

Did you find any wrong information? Please help fix it. I'm not an expert on the Oil & Gas industry neither a
mathematician. Therefore, the terms and descriptions might not be very accurate and I'd be happy to receive reviews and
contribution to this article. Please comment below.

[neuroscience]: https://www.frontiersin.org/articles/10.3389/fninf.2015.00028/full

[repository]: https://github.com/rwanderc/spikes

[telemetry]: https://glossary.oilfield.slb.com/en/terms/t/telemetry

[wellconstructionvideo]: https://www.youtube.com/watch?v=HHip4mkTrQs

---

https://www.google.com/search?q=derivative+angle+curve+discrete+series&ei=dVdoYqzIGu_-7_UP596u6Ac&ved=0ahUKEwjs1o6EyrL3AhVv_7sIHWevC30Q4dUDCA4&uact=5&oq=derivative+angle+curve+discrete+series&gs_lcp=Cgdnd3Mtd2l6EAMyBQghEKABOgcIABBHELADOgcIIRAKEKABSgQIQRgASgQIRhgAUN8EWI4JYOMLaAFwAXgBgAGGAYgBnwWSAQM2LjGYAQCgAQHIAQfAAQE&sclient=gws-wiz

https://page.math.tu-berlin.de/~techter/lecture_notes/ss14-geometry2-ddg.pdf