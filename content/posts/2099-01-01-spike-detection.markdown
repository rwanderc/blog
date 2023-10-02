---
layout:      "post"
url:         "/spike-generator-background/"
title:       "Spike detection: Background"
subtitle:    "Simulation and detection of spikes in real-time data streams"
date:        "2022-08-26 09:00:00"
author:      "Wander Costa"
header_img:  "/img/post-spikes.png"
thumb_img:   "/img/post-thumb-spikes.jpg"

tags:

- software development
- engineering management
- spikes

---

If you are a "show me code" person, jump directly to the [repository][repository]. Otherwise, enjoy the explanation of
the mathematical background. Note that the intention is not to make mathematical proofs or be mathematically rigorous,
but rather offer a concise train of thoughts to the understanding of how to spot spikes.

![](/img/spikes-simulator-sampling.png)

Detecting spikes or valleys in discrete series of data is a matter of identifying a big variation in the
**acceleration** in comparison to the previous tendency, associated with a few other factors. In continuous functions,
identifying acceleration is the next step after identifying **speed** and they can be given by the first and second
derivatives of the function, respectively. In discrete series of data, the analog concept can be applied.

# Speed of a function

The speed of a function in a specific point is given by its first derivative, and can ultimately be used to calculate the
angle of the function in that specific point, i.e. defining its tangent in the specific point.
The parabola below is described by the function `f(x) = x^2`, while its first derivative function is expressed by
`f'(x) = 2x`.
From that derivative function, it's possible to calculate the exact angle of the function in any point by the expression
`g(x) = atan(f'(x))` (in radians) or `h(x) = atan(f'(x)) * 180/π` (in degrees).

![](/img/spikes-xx-2x-angle.png)

Looking to the graph of `f(x)`, it's easy to find the point where the function changed its direction, which is also
reflected in the graph for `f'(x)`, since its value changes from negative to positive passing through `0`.

It's also interesting to observe the relation between `f(x)` and `g(x)` in the limits:
- when `x` tends to `-∞` --> `g(x)` tends to `-90`
- when `x` tends to `+∞` --> `g(x)` tends to `90`
- however, `g(x)` never touches `-90` or `90`

And it's intuitive to read this from `f(x)`, since having angles of either `-90` or `90` would mean `f(x)` would be a
vertical line, therefore having infinite values for the same `x`.

For this work, it's interesting to understand the relation of the first derivative, the angle and the curve, but it
won't be necessary to calculate angles in radians or degrees. Therefore, keep focusing on the derivatives only.

# Acceleration of a function

The acceleration of a function is the change of it's speed over the change of time. It can be interpreted as an extension
of the concept of the speed, however given by the second derivative.
For the function `f(x) = x^2`, the first derivative is given by the expression `f'(x) = 2x` while
the second derivative is given by the expression `f''(x) = 2`, which is a constant. In other words, `f(x)` has constant
acceleration for every value of `x`.

![](/img/spikes-xx-2x-2.png)

An important aspect of the acceleration is directly inferred from its signal: a positive acceleration `f''(x)` means
that `f(x)` is concave upwards; a negative acceleration `f''(x)` means that `f(x)` is concave downward. An example of
negative acceleration could be given by `f(x) = -x^2`, where `f''(x) = -2`. For both examples, however, the acceleration
is constant.

> The graph of a function with a positive second derivative is upwardly concave, while the graph of a function with a
> negative second derivative curves in the opposite way. https://en.wikipedia.org/wiki/Second_derivative

![](/img/spikes-xx-2x-2-negative.png)

Take another example function and its derivatives with a different behaviour: `f(x) = x^3 - 15x`, `f'(x) = 3x^2 - 15`
and `f''(x) = 6x`. From the second graph, it's easy to spot the 2 moments where `f'(x) = 0`, which is the reflection
of `f(x)` changing direction from increasing to decreasing or vice-versa, i.e. the speed reached `f'(x) = 0` for
both `x = sqrt(5)` and `x = -sqrt(5)`.

![](/img/spikes-xxx-3x2-6x.png)

The graph of the acceleration `f''(x)` does not provide information about the change of the direction of `f(x)`.
However, it provides a different but very important insight for this study: the indication of the change of the curvature
of the parabola described by `f(x)`. Looking at the graph from `f''(x)`, it's easy to find where `f(x)` stopped behaving
as a parabola pointing downward and started behaving as a parabola pointing upward, since the acceleration reached `0`,
coming from negative to positive.
This is then the **point of inflection** of `f(x)`: the point where the function `f(x)` changes concavity direction.

For the scope of the algorithm, it's important to understand how to identify:

- Points where the 1st derivative changes signal
- Points of inflection, where the 2nd derivative changes signal

# Speed and Acceleration in discrete series

Discrete series of data are discontinuous by definition. Concepts like limits, derivatives, integrals are not applicable.
In fact, discrete series are not even functions, but sets of points. However, calculating speed or acceleration is analog
to how it's done in functions and is based on the differences between points. Implicitly, this also defines the straight
line between the two neighbor points. Derivatives have their analog meaning for discrete series considering this logic.

![](/img/spikes-xx-discrete.png)

![](/img/spikes-xxx-discrete.png)

The speed of discrete series of data can be calculated as the angle between two points, i.e. the quotient of the
differences between `(x0, y0)` and `(x1, y1)`: `ΔY/ΔX = (y1-y0)/(x1-x0)`.

For a series of data points `S1(x,y) = {[1,1], [2,2], [3,3]}`, the speed between 1st and 2nd points, and between 2nd and
3rd points equals `1` (`ΔY/ΔX = (2-1)/(2-1) = 1` and `ΔY/ΔX = (3-2)/(3-2) = 1`). However, for the series of data
points `S2(x,y) = {[1,1], [2,2], [3,10]}`, the speed between 1st and 2nd points is `1` (`ΔY/ΔX = (2-1)/2-1) = 1`) while
between 2nd and 3rd is `8` (`ΔY/ΔX = (10-2)/(3-2) = 8`). Then the series of speeds are
respectively `S1'(x,y) = {[1,1], [2,1]}` and `S2'(x,y) = {[1,1], [2,8]}`

![](/img/spikes-discrete-examples.png)

Like the 2nd derivative, the acceleration of the discrete series can be calculated as an extension of the speed, i.e.
the quotient of the differences between two speeds `(x'0, y'0)` and `(x'1, y'1)`: `d'Y/dX = (y'1-y'0)/(x'1-x'0)`.
Therefore, given the previous data sets, the acceleration will be given by `S1''(x,y) = {[1,0]}`
and `S2''(x,y) = {[1, 7]}`. That is, the acceleration of S1 is `0` while the acceleration of S2 is `7` for the sets
specified.

With another example `S3(x,y) = {[1,1], [2,2], [3,5], [4,14]}` however, the acceleration would be given
by `S3''(x,y) = {[1,1], [2,8]}` which is not a constant anymore. The same concepts and effects of the change of signal
of the "first derivative" or the point of inflection are applied.

# Logic of the Algorithm

WIP

[neuroscience]: https://www.frontiersin.org/articles/10.3389/fninf.2015.00028/full

[pre-salt]: https://en.wikipedia.org/wiki/Pre-salt_layer

[repository]: https://github.com/rwanderc/spikes

[telemetry]: https://glossary.oilfield.slb.com/en/terms/t/telemetry

[wellconstructionvideo]: https://www.youtube.com/watch?v=HHip4mkTrQs

[patrick]: https://www.linkedin.com/in/patrick-ponte/

[next-post]: /spike-detection-algorithm
