---
layout:      post
title:       "Java - Bit Shift & Bitwise"
subtitle:    "straightforward operations over binary"
description: "Using bitwise and bit shift operators in Java to execute straightforward binary calculations."
date:        2015-12-23 05:56:00
author:      "Wander Costa"
header-img: "img/post-bg-my-development-environment.jpg"
---

Bitwise and Bit Shift are operators that make **binary calculations** perfectly straightforward.

Until few months ago, I had never used bitwise operators, and my impression is that both are indeed not widely known by developers. However they provide a great must-know solution for specific problems, like network calculations (IP & netmask); privileges processing based in bit fields; communication involving checksums, parity, flow control (e.g.: serial communication); compression; encryption, etc.

The performance is another important point. Instead of implementing more complex algorithms, binary processing can be done with the minimun effort, since bitwise and bit shift are primitive operations directly supported by processors.

The operator:

* `~` performs a bitwise complement;
* `&` performs a bitwise AND;
* `^` performs a bitwise exclusive OR;
* `|` performs a bitwise inclusive OR;
* `<<` shifts a bit pattern to the left;
* `>>` shifts a bit pattern to the right;

## Two's Complement

To understand binary operations, it's necessary to understand first how numbers are represented in binary notation. In Java Two's Complement is used to represent `byte` (8 bits long), `short` (16 bits long), `int` (32 bits long) and `long` (64 bits long) in Java.

By default, integers `int` are signed (have negative and positive values) 32 bits long, represented in two's complement, what means that:

* `0000 0000 0000 0000 0000 0000 0000 0000` represents the number 0;
* `0111 1111 1111 1111 1111 1111 1111 1111` represents the number 2147483647 ($$ 2^{31}-1 $$);
* `1000 0000 0000 0000 0000 0000 0000 0000` represents the number -2147483648 ($$ - (2^{31}) $$);
* `1111 1111 1111 1111 1111 1111 1111 1111` represents the number -1;

As you can see, the left most bit (most significant bit) defines if the number is positive (`0`) or negative (`1`). Another inference that can be done is about mirroring. If we could fold the total range in the middle, we would have:

* 0 mirroring -1;
* 1 mirroring -2;
* 2 mirroring -3, and so on, until;
* $$ 2^{31} - 1 $$ mirroring $$ - (2^{31}) $$.

**Curiosity**: note that the individual sum of all mirrored values is -1.

## `~` Bitwise Complement

Bitwise Complement `~` is the operator to binary invert the bit pattern of a value. Its result is more complex, since it also considers the left most bit of the sequence.

Let A be an integer 18:

* `0000 0000 0000 0000 0000 0000 0001 0010` represents A, which is 18;
* `1111 1111 1111 1111 1111 1111 1110 1101` represents ~A, which is -19;

Thinking about how representations work, it's reasonable, because the distance between 0 and 18 equals the distance between -1 and -19.

### `&` Bitwise AND

### `^` Exclusive OR

### `|` Inclusive OR

### `<<` Signed Left Shift

### `>>` Signed Right Shift

##Scenarios

A scenario where these operators 

`10010
 &
 01001
 =
 11011`
