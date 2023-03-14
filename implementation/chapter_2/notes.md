# Hardware Adders 🐍

Computers use two's complement to represent numbers. This is basically a scheme whereby you have a limited range of binary bit patterns and when you "overflow" so to speak you wrap around. The upshot is that addition for positive and negative numbers is the same. Converting from and to negative numbers is very easy in this scheme, you can either:

1. remove trailing least significant `0`s and the first `1` and then flip the rest OR
2. Flip all bits then add 1 to the result. When we say add 1 here I think it means the integer 1

Because of this wrapping round negation and addition can be implemented in the same hardware chip because subtraction now looks like this: x − y = x + (− y). And to get the negative of a number is easy using one of the methods outlined above.

The chips that do this stuff are usually called Arithmetic Logical Units.

## Kinds of Adders

* Half Adder - adds 2 bits only
* Full adder - adds 3 bits only
* Adder - you guessed it adds two n-bit numbers.

Often you also get an `incrementer` which just adds 1 to things (I'm guessing this'll be used for the 2. method of getting the negative of a number? Not sure.)

### Half Adder

The output of a half-adder is two bits. The LSB (least significant bit) is called the SUM and the MSB (most significant bit) is called the CARRY.

Inputs|   Outputs   |
A | B | CARYY | SUM |
---------------------
0 | 0 |  0    |  0
0 | 1 |  0    |  1
1 | 0 |  0    |  1
1 | 1 |  1    |  0

Strikes me that the CARRY is and AND ? and the SUM is what... "x * not(y) + not(x) * y + not(x) * not(y)" which I think is an exclusive OR. We should have documented better.

# Full Adder

This takes in three inputs and still only produces two outputs - the SUM and CARRY.





