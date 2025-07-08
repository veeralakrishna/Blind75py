
# 8. Search in Rotated Sorted Array

## Problem Statement

There is an integer array `nums` sorted in ascending order (with **distinct** values).

Prior to being passed to your function, `nums` is **possibly rotated** at an unknown pivot index `k` (`1 <= k < nums.length`) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (0-indexed). For example, `[0,1,2,4,5,6,7]` might be rotated at pivot index `3` and become `[4,5,6,7,0,1,2]`.

Given the array `nums` **after** the possible rotation and an integer `target`, return *the index of `target` if it is in `nums`, or `-1` if it is not in `nums`*.

You must write an algorithm with `O(log n)` runtime complexity.

**Example 1:**

**Input:** nums = [4,5,6,7,0,1,2], target = 0
**Output:** 4

**Example 2:**

**Input:** nums = [4,5,6,7,0,1,2], target = 3
**Output:** -1

**Example 3:**

**Input:** nums = [1], target = 0
**Output:** -1

## Solution

```python
class Solution:
    def search(self, nums: list[int], target: int) -> int:
        left, right = 0, len(nums) - 1

        while left <= right:
            mid = (left + right) // 2

            if nums[mid] == target:
                return mid

            # Check if the left half is sorted
            if nums[left] <= nums[mid]:
                if nums[left] <= target < nums[mid]:
                    right = mid - 1
                else:
                    left = mid + 1
            # Otherwise, the right half is sorted
            else:
                if nums[mid] < target <= nums[right]:
                    left = mid + 1
                else:
                    right = mid - 1

        return -1
```

## Explanation

This problem requires a modified binary search. In each step, we need to determine which half of the array is sorted and if the `target` lies within that sorted half.

1.  We calculate the middle index `mid`.
2.  If `nums[mid]` is the `target`, we return `mid`.
3.  We check if the left half (from `left` to `mid`) is sorted. This is true if `nums[left] <= nums[mid]`.
    -   If the left half is sorted, we check if the `target` is within the range `[nums[left], nums[mid])`. If yes, we search in the left half. Otherwise, we search in the right half.
4.  If the left half is not sorted, it means the right half (from `mid` to `right`) must be sorted.
    -   We check if the `target` is within the range `(nums[mid], nums[right]]`. If yes, we search in the right half. Otherwise, we search in the left half.

This process continues until the `target` is found or the search space is exhausted. The time complexity is `O(log n)` due to the binary search, and the space complexity is `O(1)`.
