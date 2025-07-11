
# 12. Number of 1 Bits

## Problem Statement

Write a function that takes an unsigned integer and returns the number of '1' bits it has (also known as the Hamming weight).

**Note:**

- Note that in some languages, such as Java, there is no unsigned integer type. In this case, the input will be given as a signed integer type. It should not affect your implementation, as the integer's internal binary representation is the same, whether it is signed or unsigned.
- In Java, the compiler represents the signed integers using 2's complement notation. Therefore, in Example 3, the input represents the signed integer. -3.

**Example 1:**

**Input:** n = 00000000000000000000000000001011
**Output:** 3
**Explanation:** The input binary string **00000000000000000000000000001011** has a total of three '1' bits.

**Example 2:**

**Input:** n = 00000000000000000000000010000000
**Output:** 1
**Explanation:** The input binary string **00000000000000000000000010000000** has a total of one '1' bit.

**Example 3:**

**Input:** n = 11111111111111111111111111111101
**Output:** 31
**Explanation:** The input binary string **11111111111111111111111111111101** has a total of thirty one '1' bits.

## Solution

```python
class Solution:
    def hammingWeight(self, n: int) -> int:
        count = 0
        while n != 0:
            # This operation removes the rightmost '1' bit
            n = n & (n - 1)
            count += 1
        return count
```

## Explanation

This clever bit manipulation trick provides an efficient way to count the number of set bits.

The expression `n & (n - 1)` always flips the least significant '1' bit of `n` to '0'.

Let's see an example: `n = 12` (binary `1100`)

1.  `n = 1100`, `n - 1 = 1011`. `n & (n - 1) = 1000`. `count` is 1.
2.  `n = 1000`, `n - 1 = 0111`. `n & (n - 1) = 0000`. `count` is 2.

The loop continues until `n` becomes 0. The number of times the loop runs is equal to the number of '1' bits in the original number.

This method is often more efficient than checking each bit one by one, as its runtime depends on the number of set bits, not the total number of bits.
