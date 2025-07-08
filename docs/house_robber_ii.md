
# 23. House Robber II

## Problem Statement

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are **arranged in a circle**. That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given an integer array `nums` representing the amount of money of each house, return *the maximum amount of money you can rob tonight **without alerting the police***.

**Example 1:**

**Input:** nums = [2,3,2]
**Output:** 3
**Explanation:** You cannot rob house 1 (money = 2) and then rob house 3 (money = 2), because they are adjacent houses.

**Example 2:**

**Input:** nums = [1,2,3,1]
**Output:** 4
**Explanation:** Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.

**Example 3:**

**Input:** nums = [1,2,3]
**Output:** 3

## Solution

```python
class Solution:
    def rob(self, nums: list[int]) -> int:
        if len(nums) == 1:
            return nums[0]

        # Helper function for the original House Robber problem
        def _rob(sub_nums):
            rob1, rob2 = 0, 0
            for n in sub_nums:
                temp = max(n + rob1, rob2)
                rob1 = rob2
                rob2 = temp
            return rob2

        # The problem can be broken down into two subproblems:
        # 1. Rob houses from index 0 to n-2 (excluding the last house).
        # 2. Rob houses from index 1 to n-1 (excluding the first house).
        # The maximum of these two will be the answer.
        return max(_rob(nums[:-1]), _rob(nums[1:]))

```

## Explanation

This problem is a variation of the original "House Robber" problem, with the added constraint that the houses are in a circle. This means the first and last houses are now considered adjacent.

The circular arrangement creates a new dependency: if you rob the first house, you cannot rob the last house, and vice versa.

We can break this problem down into two separate, linear subproblems:

1.  **Case 1: Exclude the last house.** We calculate the maximum amount of money that can be robbed from the houses `nums[0]` to `nums[n-2]`.
2.  **Case 2: Exclude the first house.** We calculate the maximum amount of money that can be robbed from the houses `nums[1]` to `nums[n-1]`.

The final answer is the maximum of the results from these two cases. This covers all possibilities while respecting the circular constraint.

We can reuse the O(1) space solution from the original House Robber problem to solve each subproblem. The overall time complexity will be O(n) because we are essentially running the linear scan twice on subarrays. The space complexity is O(1).
