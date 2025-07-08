
# 11. Sum of Two Integers

## Problem Statement

Given two integers `a` and `b`, return *the sum of the two integers without using the operators `+` and `-`*.

**Example 1:**

**Input:** a = 1, b = 2
**Output:** 3

**Example 2:**

**Input:** a = 2, b = 3
**Output:** 5

## Solution

```python
class Solution:
    def getSum(self, a: int, b: int) -> int:
        # 32-bit mask in hexadecimal
        mask = 0xffffffff

        # In Python, integers have unlimited precision, so we need to handle
        # negative numbers and overflow manually by using a mask.
        while (b & mask) > 0:
            carry = (a & b) << 1
            a = a ^ b
            b = carry

        # If the result is negative, it will be stored in two's complement
        # format. We need to convert it back to a standard negative integer.
        return (a & mask) if b > 0 else a
```

## Explanation

This problem requires us to implement addition using bitwise operations. The core idea is to simulate the way a computer's hardware adder works.

We use two main bitwise operators:

-   **XOR (`^`):** This gives the sum without considering the carry. For example, `1 ^ 1 = 0`, `1 ^ 0 = 1`.
-   **AND (`&`) and Left Shift (`<<`):** This combination calculates the carry. `(a & b)` finds the bits where both `a` and `b` are 1, which is where a carry is generated. `<< 1` shifts this carry to the next significant bit position.

The algorithm works as follows:

1.  Calculate the sum without carry (`a ^ b`).
2.  Calculate the carry (`(a & b) << 1`).
3.  The new `a` becomes the sum from step 1, and the new `b` becomes the carry from step 2.
4.  We repeat this process until there are no more carries (`b` becomes 0).

**Handling Python's Integers:** Python's integers can be arbitrarily large, which can cause issues with negative numbers in bitwise operations. We use a `mask` ( `0xffffffff` for 32-bit integers) to ensure the operations behave as they would on fixed-size integers, correctly handling overflow and two's complement for negative numbers.
