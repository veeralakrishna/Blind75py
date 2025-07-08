
# 4. Product of Array Except Self

## Problem Statement

Given an integer array `nums`, return *an array `answer` such that `answer[i]` is equal to the product of all the elements of `nums` except `nums[i]`*.

The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.

You must write an algorithm that runs in O(n) time and without using the division operation.

**Example 1:**

**Input:** nums = [1,2,3,4]
**Output:** [24,12,8,6]

**Example 2:**

**Input:** nums = [-1,1,0,-3,3]
**Output:** [0,0,9,0,0]

## Solution

```python
class Solution:
    def productExceptSelf(self, nums: list[int]) -> list[int]:
        n = len(nums)
        answer = [1] * n

        # Calculate left products
        left_product = 1
        for i in range(n):
            answer[i] = left_product
            left_product *= nums[i]

        # Calculate right products and multiply
        right_product = 1
        for i in range(n - 1, -1, -1):
            answer[i] *= right_product
            right_product *= nums[i]

        return answer
```

## Explanation

This solution avoids division and achieves O(n) time complexity by using a two-pass approach.

1.  **First Pass (Left Products):** We create an `answer` array of the same size as `nums`, initialized with 1s. We iterate from left to right. For each position `i`, we set `answer[i]` to the product of all elements to its left. We use a `left_product` variable to keep track of this running product.

2.  **Second Pass (Right Products):** We then iterate from right to left. This time, we use a `right_product` variable to track the product of elements to the right of the current position. We multiply the existing `answer[i]` (which already holds the left product) by the `right_product` to get the final result.

This method cleverly calculates the total product except for the element at the current index by combining the products of its left and right sides. The space complexity is O(1) if we don't count the output array, as we are modifying it in place.
