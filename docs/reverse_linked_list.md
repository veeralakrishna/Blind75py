
# 44. Reverse Linked List

## Problem Statement

Given the `head` of a singly linked list, reverse the list, and return *the reversed list*.

**Example 1:**

![Reverse Linked List Example 1](https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg)

**Input:** head = [1,2,3,4,5]
**Output:** [5,4,3,2,1]

**Example 2:**

![Reverse Linked List Example 2](https://assets.leetcode.com/uploads/2021/02/19/rev1ex2.jpg)

**Input:** head = [1,2]
**Output:** [2,1]

**Example 3:**

**Input:** head = []
**Output:** []

## Solution

```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        prev = None
        curr = head

        while curr:
            # Store the next node
            next_temp = curr.next
            # Reverse the current node's pointer
            curr.next = prev
            # Move pointers one step forward
            prev = curr
            curr = next_temp

        return prev
```

## Explanation

This problem is a classic linked list manipulation task. The goal is to reverse the direction of the pointers in the list.

**Iterative Approach:**

We use three pointers:

1.  `prev`: Initially `None`. This pointer will eventually become the new head of the reversed list.
2.  `curr`: Initially `head`. This pointer iterates through the original list.
3.  `next_temp`: A temporary pointer to store `curr.next` before `curr.next` is changed.

**Algorithm:**

-   We iterate while `curr` is not `None`:
    -   **Step 1: Store `next_temp`:** Save the `curr.next` node in `next_temp`. This is crucial because we are about to change `curr.next`, and we need to retain the link to the rest of the original list.
    -   **Step 2: Reverse Pointer:** Change `curr.next` to point to `prev`. This is the actual reversal step.
    -   **Step 3: Advance `prev`:** Move `prev` to `curr`. The `curr` node has now been processed and becomes the new `prev` for the next iteration.
    -   **Step 4: Advance `curr`:** Move `curr` to `next_temp`. This moves `curr` to the next node in the original list.

-   When the loop finishes, `curr` will be `None`, and `prev` will be pointing to the last node of the original list, which is now the head of the reversed list.

**Time and Space Complexity:**

-   **Time Complexity:** O(n), where n is the number of nodes in the linked list, as we iterate through the list once.
-   **Space Complexity:** O(1), as we only use a few extra pointers.
