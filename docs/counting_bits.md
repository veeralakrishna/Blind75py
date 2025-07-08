
# 13. Counting Bits

## Problem Statement

Given an integer `n`, return *an array `ans` of length `n + 1` such that for each `i` (`0 <= i <= n`), `ans[i]` is the **number of 1's** in the binary representation of `i`*.

**Example 1:**

**Input:** n = 2
**Output:** [0,1,1]
**Explanation:**
0 --> 0
1 --> 1
2 --> 10

**Example 2:**

**Input:** n = 5
**Output:** [0,1,1,2,1,2]
**Explanation:**
0 --> 0
1 --> 1
2 --> 10
3 --> 11
4 --> 100
5 --> 101

## Solution

```python
class Solution:
    def countBits(self, n: int) -> list[int]:
        # Initialize an array to store the results
        ans = [0] * (n + 1)

        # Iterate from 1 to n
        for i in range(1, n + 1):
            # The number of 1s in i is the number of 1s in i // 2 (i >> 1)
            # plus 1 if i is odd.
            ans[i] = ans[i >> 1] + (i & 1)

        return ans
```

## Explanation

This problem can be solved using dynamic programming by observing a pattern in the number of set bits.

The number of '1's in the binary representation of an integer `i` can be derived from a previously computed value.

Let `ans[i]` be the number of set bits in `i`.

-   **Right shifting `i` by 1 (`i >> 1`)** is equivalent to integer division by 2. This effectively removes the least significant bit.
-   **Checking `i & 1`** tells us if `i` is odd or even. If `i` is odd, its least significant bit is 1. If `i` is even, it's 0.

Therefore, the number of set bits in `i` is the same as the number of set bits in `i // 2`, plus an additional 1 if `i` is odd.

This leads to the recurrence relation: `ans[i] = ans[i >> 1] + (i & 1)`.

We can build up the `ans` array from 0 to `n` using this relation, resulting in an O(n) time complexity and O(n) space complexity for the output array.
