
# 15. Reverse Bits

## Problem Statement

Reverse bits of a given 32 bits unsigned integer.

**Note:**

- Note that in some languages like Java, there is no unsigned integer type. In this case, both input and output will be given as a signed integer type. They should not affect your implementation, as the integer's internal binary representation is the same, whether it is signed or unsigned.
- In Java, the compiler represents the signed integers using 2's complement notation. Therefore, in Example 2, the input represents the signed integer -3 and the output represents the signed integer -1073741825.

**Example 1:**

**Input:** n = 00000010100101000001111010011100
**Output:** 00111001011110000010100101000000

**Example 2:**

**Input:** n = 11111111111111111111111111111101
**Output:** 10111111111111111111111111111111

## Solution

```python
class Solution:
    def reverseBits(self, n: int) -> int:
        res = 0
        for i in range(32):
            # Left shift the result to make space for the next bit
            res <<= 1
            # Get the least significant bit of n
            bit = n & 1
            # Add the bit to the result
            res |= bit
            # Right shift n to process the next bit
            n >>= 1
        return res
```

## Explanation

This solution reverses the bits of a 32-bit integer by iterating 32 times, once for each bit.

In each iteration, we do the following:

1.  **`res <<= 1`:** We make space in our `res` (result) variable for the next bit by shifting it to the left.
2.  **`bit = n & 1`:** We extract the least significant bit (LSB) of the input `n` using the AND operator with 1.
3.  **`res |= bit`:** We add this extracted bit to our result using the OR operator.
4.  **`n >>= 1`:** We shift the input `n` to the right to discard the LSB and expose the next bit for the following iteration.

This process effectively builds the reversed number bit by bit, from right to left in the original number, which becomes left to right in the new number.

The time complexity is O(1) because the loop runs a fixed 32 times, regardless of the input value. The space complexity is also O(1).
