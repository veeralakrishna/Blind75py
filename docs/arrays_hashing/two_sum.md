
# 1. Two Sum

## Problem Statement

Given an array of integers `nums` and an integer `target`, return *indices of the two numbers such that they add up to `target`*.

You may assume that each input would have **exactly one solution**, and you may not use the *same* element twice.

You can return the answer in any order.

**Example 1:**

**Input:** nums = [2,7,11,15], target = 9
**Output:** [0,1]
**Explanation:** Because nums[0] + nums[1] == 9, we return [0, 1].

**Example 2:**

**Input:** nums = [3,2,4], target = 6
**Output:** [1,2]

**Example 3:**

**Input:** nums = [3,3], target = 6
**Output:** [0,1]

## Solution

```python
class Solution:
    def twoSum(self, nums: list[int], target: int) -> list[int]:
        num_to_index = {}
        for i, num in enumerate(nums):
            complement = target - num
            if complement in num_to_index:
                return [num_to_index[complement], i]
            num_to_index[num] = i
```

## Explanation

The solution uses a hash map (dictionary in Python) to store the numbers we've seen so far and their indices.

We iterate through the `nums` array. For each number, we calculate its `complement` (the number that would add up to the `target`).

If the `complement` is already in our hash map, it means we've found our pair. We return the index of the `complement` (from the hash map) and the index of the current number.

If the `complement` is not in the hash map, we add the current number and its index to the map and continue to the next number.

This approach has a time complexity of O(n) because we iterate through the array once. The space complexity is also O(n) for the hash map.
