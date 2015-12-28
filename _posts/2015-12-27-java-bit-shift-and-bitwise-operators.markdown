---
layout:      post
title:       "Bit Shift & Bitwise"
subtitle:    "straightforward operations over binary in Java"
description: "Using bitwise and bit shift operators in Java to execute straightforward binary calculations."
date:        2015-12-27 20:10:00
author:      "Wander Costa"
header-img:  "/img/post-bg-binary.jpg"
thumb-img:   "/img/thumb/thumb-java-bit-shift-and-bitwise-operators.jpg"
---

**Bit Shift** and **Bitwise** are operators that make **binary calculations** perfectly straightforward.

Until few months ago, I had never used bitwise operators, and my impression is that both are indeed not even known by most developers.

These operators provide a great must-know solution for specific problems, like network calculations (IP & netmask); privileges processing based in bit fields; communication involving checksums, parity, flow control (e.g.: serial communication); compression; encryption, etc.

The performance is another important point. Instead of implementing more complex algorithms, binary processing can be done with the minimun effort, since **bitwise** and **bit shift** are primitive operations **directly supported by processors**.

The operators are:

* `~`, which performs a bitwise complement;
* `&`, which performs a bitwise AND;
* `^`, which performs a bitwise exclusive OR;
* `|`, which performs a bitwise inclusive OR;
* `<<`, which shifts a bit pattern to the left;
* `>>`, which shifts a bit pattern to the right;
* `>>>`, which shifts a zero into the leftmost position.

### Two's Complement

To understand what are binary operations, it's necessary to understand how signed integer numbers are represented in binary notation. My previous post, [Two's Complement][twos-complement], describes it.

### `~` Bitwise Complement

Bitwise Complement `~` is the operator to binary invert the bit pattern of a value.

Let `A` be an `int` with value `18`:

* `0000 0000 0000 0000 0000 0000 0001 0010` represents A, which is 18;
* `1111 1111 1111 1111 1111 1111 1110 1101` represents ~A, which is -19 in [Two's Complement][twos-complement].

All bits were inverted, thus turning `0` to `1` and `1` to `0`. Because of the [Two's Complement][twos-complement] architecture, every ~X will be $$-X - 1$$.

### `&` Bitwise AND

Bitwise AND `&` is the operator to execute binary conjunction.

Let `A` be a `short` with value `18`, and `B` be a `short` with value `4216`;

* `0000 0000 0001 0010` represents A, which is 18;
* `0001 0000 0111 1000` represents B, which is 4216;
* `0000 0000 0001 0000` represents A & B, which is 16.

Binary conjunction turns two `1` into `1`, and anything else into `0`. In the above example, the only coincident bit in `A` and `B` was the 5th bit (from right to left), what makes the result value be `16`.

### `^` Bitwise Exclusive OR

Bitwise Exclusive OR `^` is the operator to execute binary exclusive disjunctions, also known as **XOR**.

Using the same previous example, let `A` be a `short` with value `18`, and `B` be a `short` with value `4216`;

* `0000 0000 0001 0010` represents A, which is 18;
* `0001 0000 0111 1000` represents B, which is 4216;
* `0001 0000 0110 1010` represents A ^ B, which is 4202.

Binary exclusive disjunction turns one, and only one, `1` into `1`, and anything else into `0`. In the above example, the result value was 4202.

### `|` Bitwise Inclusive OR

Bitwise Inclusive OR `|` is the operator to execute binary inclusive disjunctions, also known as simply **OR**.

Using the same previous example, let `A` be a `short` with value `18`, and `B` be a `short` with value `4216`;

* `0000 0000 0001 0010` represents A, which is 18;
* `0001 0000 0111 1000` represents B, which is 4216;
* `0001 0000 0111 1010` represents A ^ B, which is 4218.

Binary inclusive disjunction turns two `0` into `0` and anything else into `1`. In the above example, the result value was 4218.

### `<<` Signed Left Shift

Signed Left Shift `<<` is the operator to shift bits to the left.

Let `A` be a `short` with value `25`.

* `0000 0000 0001 1001` represents A, which is 25;
* `0000 0000 0110 0100` represents A << 2, which is 100.

The operation above shifted 2 positions to the left, making the result value be 100.

It's important to understand what happens the in most significant bit, and to understand it, let's take another example.

Let `B` be a `short` with value `8192`.

* `0010 0000 0000 0000` represents B, which is 8192;
* `0100 0000 0000 0000` represents B << 1, which is 16384;
* `1000 0000 0000 0000` represents B << 2, which is -32768 in [Two's Complement][twos-complement].
* `0000 0000 0000 0000` represents B << 3, which is 0.

That is, simple shifts. No special behavior happens during shift to the left.

### `>>` Signed Right Shift

Signed Right Shift `>>` is the operator to shift bits to the right, keeping the signal.

Let `A` be a `short` with value `25`.

* `0000 0000 0001 1001` represents A, which is 25;
* `0000 0000 0000 0110` represents A >> 2, which is 12.

The operation above shifted the bits 2 positions to the right, making the result value be 12.

To figure out what happens in the most significant bit, let's also take other examples.

Let `B` be a `short` with value `16384`.

* `0100 0000 0000 0000` represents B, which is 16384;
* `0010 0000 0000 0000` represents B >> 1, which is 8192.

Let `C` be a `short` with value `-32768`.

* `1000 0000 0000 0000` represents C, which is -32768;
* `1100 0000 0000 0000` represents B >> 1, which is -16384;
* `1110 0000 0000 0000` represents B >> 1, which is -8192;

Thus, we conclude that Signed Right Shift keeps the signal. In other words, it shifts a bit at the leftmost position with the same value as the signal.

### `>>>` Unsigned Right Shift

Unsigned Right Shift `>>>` is the operator to shift bits to the right, not considering the signal.

The behavior is the same as the Signed Right Shift, with only an excepion: the bit added to the left is ALWAYS zero. Let's take the last example of Signed Right Shift:

Let `C` be a `short` with value `-32768`.

* `1000 0000 0000 0000` represents C, which is -32768;
* `0100 0000 0000 0000` represents B >> 1, which is 16384;
* `0010 0000 0000 0000` represents B >> 1, which is 8192;

It results in always producing a positive value, which is the square root of the absolute value of C.

### Shift Peculiarities

Shifts in binary representations are shortcuts to calculate powers of 2 and square roots, ONLY IF it does not modify the most significant bit.

When shifting A to the left in one position, if the most significant bit is not modified, the result will be $$ A^2 $$. When shifting A to the right in one position, if the most significant bit is not modified, the result will be $$ \sqrt A $$.

[twos-complement]: twos-complement
[unix-permissions]: https://en.wikipedia.org/wiki/File_system_permissions
