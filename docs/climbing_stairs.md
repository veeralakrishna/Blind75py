
# 16. Climbing Stairs

## Problem Statement

You are climbing a staircase. It takes `n` steps to reach the top.

Each time you can either climb `1` or `2` steps. In how many distinct ways can you climb to the top?

**Example 1:**

**Input:** n = 2
**Output:** 2
**Explanation:** There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps

**Example 2:**

**Input:** n = 3
**Output:** 3
**Explanation:** There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step

## Solution

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        if n <= 2:
            return n

        # We only need to store the last two values
        one_step_before = 2
        two_steps_before = 1
        all_ways = 0

        for _ in range(2, n):
            all_ways = one_step_before + two_steps_before
            two_steps_before = one_step_before
            one_step_before = all_ways

        return all_ways
```

## Explanation

This is a classic dynamic programming problem that turns out to be the Fibonacci sequence.

Let `dp[i]` be the number of ways to reach the `i`-th step.

- To reach step `i`, you could have come from step `i-1` (by taking one step) or from step `i-2` (by taking two steps).
- Therefore, the total number of ways to reach step `i` is the sum of the ways to reach step `i-1` and the ways to reach step `i-2`.

This gives us the recurrence relation: `dp[i] = dp[i-1] + dp[i-2]`.

This is the same as the Fibonacci sequence.

- `dp[1] = 1` (1 way to reach the first step)
- `dp[2] = 2` (2 ways: 1+1 or 2)
- `dp[3] = dp[2] + dp[1] = 2 + 1 = 3`

The provided solution is an optimized version of this. Instead of storing the entire `dp` array, we only need to keep track of the last two values (`one_step_before` and `two_steps_before`), which correspond to `dp[i-1]` and `dp[i-2]`.

This reduces the space complexity to O(1) while maintaining an O(n) time complexity.
