
# 7. Find Minimum in Rotated Sorted Array

## Problem Statement

Suppose an array of length `n` sorted in ascending order is **rotated** between `1` and `n` times. For example, the array `nums = [0,1,2,4,5,6,7]` might become:

- `[4,5,6,7,0,1,2]` if it was rotated `4` times.
- `[0,1,2,4,5,6,7]` if it was rotated `7` times.

Notice that **rotating** an array `[a[0], a[1], a[2], ..., a[n-1]]` 1 time results in the array `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]`.

Given the sorted rotated array `nums` of **unique** elements, return *the minimum element of this array*.

You must write an algorithm that runs in `O(log n)` time.

**Example 1:**

**Input:** nums = [3,4,5,1,2]
**Output:** 1
**Explanation:** The original array was [1,2,3,4,5] rotated 3 times.

**Example 2:**

**Input:** nums = [4,5,6,7,0,1,2]
**Output:** 0
**Explanation:** The original array was [0,1,2,4,5,6,7] and it was rotated 4 times.

**Example 3:**

**Input:** nums = [11,13,15,17]
**Output:** 11
**Explanation:** The original array was [11,13,15,17] and it was rotated 4 times.

## Solution

```python
class Solution:
    def findMin(self, nums: list[int]) -> int:
        left, right = 0, len(nums) - 1

        while left < right:
            mid = (left + right) // 2

            if nums[mid] > nums[right]:
                # The pivot (minimum element) is in the right half
                left = mid + 1
            else:
                # The pivot is in the left half or is the mid element
                right = mid

        return nums[left]
```

## Explanation

This problem can be solved efficiently using a modified binary search. The key insight is to compare the middle element with the rightmost element to determine which half of the array contains the minimum element (the pivot point).

- If `nums[mid] > nums[right]`, it means the rotation pivot must be in the right half of the array (from `mid + 1` to `right`). The left part is sorted correctly.
- If `nums[mid] <= nums[right]`, the pivot is either the middle element itself or in the left half. So, we search in the left half, including the middle element.

We continue narrowing down the search space until `left` and `right` converge. The element at this final index is the minimum element of the rotated array.

This binary search approach guarantees an `O(log n)` time complexity with `O(1)` space complexity.
