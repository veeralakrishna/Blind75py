
# 9. 3Sum

## Problem Statement

Given an integer array `nums`, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

Notice that the solution set must not contain duplicate triplets.

**Example 1:**

**Input:** nums = [-1,0,1,2,-1,-4]
**Output:** [[-1,-1,2],[-1,0,1]]
**Explanation:**
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0.
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0.
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0.
The distinct triplets are [-1,0,1] and [-1,-1,2].
Notice that the order of the output and the order of the triplets does not matter.

**Example 2:**

**Input:** nums = [0,1,1]
**Output:** []
**Explanation:** The only possible triplet does not sum up to 0.

**Example 3:**

**Input:** nums = [0,0,0]
**Output:** [[0,0,0]]
**Explanation:** The only possible triplet sums up to 0.

## Solution

```python
class Solution:
    def threeSum(self, nums: list[int]) -> list[list[int]]:
        nums.sort()
        result = []
        n = len(nums)

        for i in range(n - 2):
            # Skip duplicate elements for the first number
            if i > 0 and nums[i] == nums[i - 1]:
                continue

            left, right = i + 1, n - 1
            while left < right:
                total = nums[i] + nums[left] + nums[right]

                if total < 0:
                    left += 1
                elif total > 0:
                    right -= 1
                else:
                    result.append([nums[i], nums[left], nums[right]])
                    # Skip duplicate elements for the second and third numbers
                    while left < right and nums[left] == nums[left + 1]:
                        left += 1
                    while left < right and nums[right] == nums[right - 1]:
                        right -= 1
                    left += 1
                    right -= 1

        return result
```

## Explanation

This solution efficiently finds all unique triplets that sum to zero by first sorting the array. Sorting allows us to use the two-pointer technique.

1.  **Sort the array:** This is crucial for the two-pointer approach and for handling duplicates.
2.  **Iterate through the array:** We use a loop to fix the first number of the triplet (`nums[i]`).
3.  **Two-Pointer Approach:** For each `nums[i]`, we use two pointers, `left` and `right`, to find the other two numbers. `left` starts at `i + 1`, and `right` starts at the end of the array.
    -   If the sum is less than zero, we need a larger sum, so we move `left` to the right.
    -   If the sum is greater than zero, we need a smaller sum, so we move `right` to the left.
    -   If the sum is zero, we've found a triplet. We add it to our result list.
4.  **Skip Duplicates:** After finding a valid triplet, we move both `left` and `right` pointers and also skip any duplicate elements to avoid adding the same triplet multiple times.

The time complexity is dominated by the sorting step, which is O(n log n). The two-pointer scan is O(n^2) in the worst case. So, the overall time complexity is O(n^2). The space complexity is O(1) (or O(n) if you count the space for the output).
