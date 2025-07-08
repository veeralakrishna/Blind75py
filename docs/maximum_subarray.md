
# 5. Maximum Subarray

## Problem Statement

Given an integer array `nums`, find the subarray with the largest sum, and return *its sum*.

**Example 1:**

**Input:** nums = [-2,1,-3,4,-1,2,1,-5,4]
**Output:** 6
**Explanation:** The subarray [4,-1,2,1] has the largest sum 6.

**Example 2:**

**Input:** nums = [1]
**Output:** 1
**Explanation:** The subarray [1] has the largest sum 1.

**Example 3:**

**Input:** nums = [5,4,-1,7,8]
**Output:** 23
**Explanation:** The subarray [5,4,-1,7,8] has the largest sum 23.

## Solution

```python
class Solution:
    def maxSubArray(self, nums: list[int]) -> int:
        max_so_far = nums[0]
        current_max = nums[0]

        for i in range(1, len(nums)):
            current_max = max(nums[i], current_max + nums[i])
            max_so_far = max(max_so_far, current_max)

        return max_so_far
```

## Explanation

This problem is a classic example of Kadane's Algorithm.

The algorithm iterates through the array, keeping track of two variables:

-   `current_max`: The maximum sum of a subarray ending at the current position.
-   `max_so_far`: The overall maximum sum found so far.

For each element, we have two choices:

1.  Start a new subarray at the current element.
2.  Extend the previous subarray by adding the current element.

We choose the one that gives a larger sum for `current_max`. Then, we update `max_so_far` if `current_max` is greater.

This dynamic programming approach efficiently finds the maximum subarray sum in O(n) time with O(1) space complexity.
