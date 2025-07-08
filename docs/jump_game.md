
# 26. Jump Game

## Problem Statement

You are given an integer array `nums`. You are initially positioned at the array's **first index**, and each element in the array represents your maximum jump length at that position.

Return `true` *if you can reach the last index, or* `false` *otherwise*.

**Example 1:**

**Input:** nums = [2,3,1,1,4]
**Output:** true
**Explanation:** Jump 1 step from index 0 to 1, then 3 steps to the last index.

**Example 2:**

**Input:** nums = [3,2,1,0,4]
**Output:** false
**Explanation:** You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.

## Solution

```python
class Solution:
    def canJump(self, nums: list[int]) -> bool:
        goal = len(nums) - 1

        # Iterate backwards from the second to last element
        for i in range(len(nums) - 2, -1, -1):
            # If we can reach the current goal from this position
            if i + nums[i] >= goal:
                # Update the goal to this position
                goal = i

        # If we managed to move the goal to the start, it's possible
        return goal == 0
```

## Explanation

This problem can be solved with a greedy approach.

The idea is to work backward from the end of the array.

1.  **Initialization:** We set our `goal` to the last index of the array (`len(nums) - 1`).

2.  **Backward Iteration:** We iterate from the second to last element (`len(nums) - 2`) down to the first element (`0`).

3.  **Greedy Choice:** In each position `i`, we check if we can reach the current `goal` from this position. This is true if `i + nums[i] >= goal`.
    -   If we can reach the `goal`, it means this position `i` is a "good" position. We can now update our `goal` to be this position `i`, because we know if we can reach `i`, we can definitely reach the end.

4.  **Final Check:** After the loop finishes, we check if the `goal` has been moved all the way to the starting index (0). If `goal == 0`, it means we found a path of "good" positions from the start to the end, so it's possible to reach the last index.

This greedy algorithm is very efficient, with a time complexity of O(n) and a space complexity of O(1).
