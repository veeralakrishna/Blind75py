
# 22. House Robber

## Problem Statement

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that **adjacent houses have security systems connected and it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given an integer array `nums` representing the amount of money of each house, return *the maximum amount of money you can rob tonight **without alerting the police***.

**Example 1:**

**Input:** nums = [1,2,3,1]
**Output:** 4
**Explanation:** Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.

**Example 2:**

**Input:** nums = [2,7,9,3,1]
**Output:** 12
**Explanation:** Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
Total amount you can rob = 2 + 9 + 1 = 12.

## Solution

```python
class Solution:
    def rob(self, nums: list[int]) -> int:
        rob1, rob2 = 0, 0

        # [rob1, rob2, n, n+1, ...]
        # rob1 is the max profit from robbing up to house i-2
        # rob2 is the max profit from robbing up to house i-1
        for n in nums:
            # The new max profit is either robbing the current house (n + rob1)
            # or not robbing it (rob2).
            temp = max(n + rob1, rob2)
            rob1 = rob2
            rob2 = temp
        return rob2
```

## Explanation

This is a classic dynamic programming problem. Let's define `dp[i]` as the maximum amount of money that can be robbed from the first `i` houses.

When considering house `i`, the robber has two choices:

1.  **Rob house `i`:** If the robber chooses to rob house `i`, they cannot rob the previous house (`i-1`). The total amount would be `nums[i] + dp[i-2]` (money from the current house plus the max money from two houses before).
2.  **Don't rob house `i`:** If the robber chooses not to rob house `i`, the maximum amount of money is simply the maximum amount they could have robbed from the previous `i-1` houses, which is `dp[i-1]`.

This gives the recurrence relation: `dp[i] = max(nums[i] + dp[i-2], dp[i-1])`.

The provided solution is a space-optimized version of this DP approach.

-   `rob2` represents the maximum profit up to the previous house (`dp[i-1]`).
-   `rob1` represents the maximum profit up to two houses before (`dp[i-2]`).

In each iteration, we calculate the new maximum profit (`temp`) and then update `rob1` and `rob2` for the next iteration. This reduces the space complexity from O(n) to O(1) while keeping the time complexity at O(n).
