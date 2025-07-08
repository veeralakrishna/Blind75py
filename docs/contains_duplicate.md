
# 3. Contains Duplicate

## Problem Statement

Given an integer array `nums`, return `true` if any value appears **at least twice** in the array, and return `false` if every element is distinct.

**Example 1:**

**Input:** nums = [1,2,3,1]
**Output:** true

**Example 2:**

**Input:** nums = [1,2,3,4]
**Output:** false

**Example 3:**

**Input:** nums = [1,1,1,3,3,4,3,2,4,2]
**Output:** true

## Solution

```python
class Solution:
    def containsDuplicate(self, nums: list[int]) -> bool:
        seen = set()
        for num in nums:
            if num in seen:
                return True
            seen.add(num)
        return False
```

## Explanation

This solution uses a hash set (`seen`) to efficiently check for duplicates.

We iterate through the input array `nums`. For each number, we check if it's already present in our `seen` set.

- If the number is in the set, we have found a duplicate and immediately return `True`.
- If the number is not in the set, we add it to the set and continue to the next element.

If we finish iterating through the entire array without finding any duplicates, it means all elements are distinct, and we return `False`.

This approach has a time complexity of O(n) because we iterate through the array once, and set lookups/insertions are O(1) on average. The space complexity is O(n) in the worst case, where all elements are unique and stored in the set.
