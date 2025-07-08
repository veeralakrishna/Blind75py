
# 51. Find the Duplicate Number

## Problem Statement

Given an array of integers `nums` containing `n + 1` integers where each integer is in the range `[1, n]` inclusive.

There is only **one repeated number** in `nums`, return *this repeated number*.

You must solve the problem **without modifying the array `nums`** and uses only constant extra space.

**Example 1:**

**Input:** nums = [1,3,4,2,2]
**Output:** 2

**Example 2:**

**Input:** nums = [3,1,3,4,2]
**Output:** 3

**Example 3:**

**Input:** nums = [3,3,3,3,3]
**Output:** 3

## Solution

```python
class Solution:
    def findDuplicate(self, nums: list[int]) -> int:
        # Phase 1: Find the intersection point of the two runners.
        # This is similar to detecting a cycle in a linked list.
        slow = nums[0]
        fast = nums[0]

        while True:
            slow = nums[slow]
            fast = nums[nums[fast]]
            if slow == fast:
                break

        # Phase 2: Find the "entrance" to the cycle.
        # Move one pointer to the start (nums[0]) and keep the other at the meeting point.
        # They will meet at the start of the cycle, which is the duplicate number.
        ptr1 = nums[0]
        ptr2 = slow

        while ptr1 != ptr2:
            ptr1 = nums[ptr1]
            ptr2 = nums[ptr2]

        return ptr1
```

## Explanation

This problem can be cleverly solved using **Floyd's Tortoise and Hare (Cycle Detection) Algorithm**, which is typically used for linked lists. The key insight is to view the array as a linked list where `nums[i]` is the next node after `i`.

Since there are `n+1` numbers in the range `[1, n]`, and exactly one number is duplicated, this creates a cycle in our conceptual linked list. For example, if `nums = [1,3,4,2,2]`, the sequence of `next` pointers would be:

`0 -> 1 -> 3 -> 2 -> 2` (a cycle at 2)

**Phase 1: Detect the Cycle**

1.  Initialize `slow` and `fast` pointers, both starting at `nums[0]`.
2.  In each step, `slow` moves one step (`slow = nums[slow]`), and `fast` moves two steps (`fast = nums[nums[fast]]`).
3.  If there's a duplicate, `slow` and `fast` will eventually meet inside the cycle. When they meet, we break the loop.

**Phase 2: Find the Start of the Cycle (the Duplicate Number)**

1.  Reset one pointer (`ptr1`) back to `nums[0]`.
2.  Keep the other pointer (`ptr2`, which is `slow` from Phase 1) at the meeting point.
3.  Move both `ptr1` and `ptr2` one step at a time (`ptr1 = nums[ptr1]`, `ptr2 = nums[ptr2]`).
4.  The point where they meet again is the entry point of the cycle, which is the duplicate number.

**Why this works:** The mathematical proof for Floyd's algorithm shows that if a cycle exists, the distance from the start of the list to the cycle entry point is equal to the distance from the meeting point to the cycle entry point. Therefore, by moving one pointer from the start and the other from the meeting point, they will converge at the cycle entry.

**Time and Space Complexity:**

-   **Time Complexity:** O(n), as both pointers traverse the array at most a constant number of times.
-   **Space Complexity:** O(1), as we only use a few extra pointers and do not modify the input array.
