
# 6. Maximum Product Subarray

## Problem Statement

Given an integer array `nums`, find a subarray that has the largest product, and return *the product*.

The test cases are generated so that the answer will fit in a **32-bit** integer.

**Example 1:**

**Input:** nums = [2,3,-2,4]
**Output:** 6
**Explanation:** [2,3] has the largest product 6.

**Example 2:**

**Input:** nums = [-2,0,-1]
**Output:** 0
**Explanation:** The result cannot be 2, because [-2,-1] is not a subarray.

## Solution

```python
class Solution:
    def maxProduct(self, nums: list[int]) -> int:
        if not nums:
            return 0

        max_prod = nums[0]
        min_prod = nums[0]
        result = max_prod

        for i in range(1, len(nums)):
            curr = nums[i]
            temp_max = max(curr, max_prod * curr, min_prod * curr)
            min_prod = min(curr, max_prod * curr, min_prod * curr)

            max_prod = temp_max

            result = max(max_prod, result)

        return result
```

## Explanation

This problem is similar to "Maximum Subarray", but with a twist: negative numbers. A negative number multiplied by another negative number becomes positive. This means we need to keep track of both the maximum and minimum product at each position.

The algorithm iterates through the array, maintaining:

-   `max_prod`: The maximum product of a subarray ending at the current position.
-   `min_prod`: The minimum product of a subarray ending at the current position (this is needed to handle negative numbers).
-   `result`: The overall maximum product found so far.

For each element, the new `max_prod` is the maximum of the current number itself, the product of the old `max_prod` and the current number, and the product of the old `min_prod` and the current number. The same logic applies to `min_prod`.

This ensures we correctly handle the sign changes and find the maximum product in O(n) time and O(1) space.
