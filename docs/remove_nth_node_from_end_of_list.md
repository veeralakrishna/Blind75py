
# 46. Remove Nth Node From End of List

## Problem Statement

Given the `head` of a linked list, remove the `n`th node from the end of the list and return its head.

**Example 1:**

![Remove Nth Node From End of List Example 1](https://assets.leetcode.com/uploads/2020/10/03/remove_nth_node_example_1.png)

**Input:** head = [1,2,3,4,5], n = 2
**Output:** [1,2,3,5]

**Example 2:**

**Input:** head = [1], n = 1
**Output:** []

**Example 3:**

**Input:** head = [1,2], n = 1
**Output:** [1]

## Solution

```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        # Create a dummy node to handle edge cases (e.g., removing the head)
        dummy = ListNode(0, head)
        first = dummy
        second = dummy

        # Move first pointer n+1 steps ahead
        # This creates a gap of n nodes between first and second
        for _ in range(n + 1):
            first = first.next

        # Move both pointers until first reaches the end
        # When first reaches the end, second will be at the node just before
        # the one to be removed.
        while first:
            first = first.next
            second = second.next

        # Remove the nth node from the end
        second.next = second.next.next

        return dummy.next
```

## Explanation

This problem can be efficiently solved using the two-pointer technique.

**Core Idea:** We want to find the node that is `n` positions away from the end of the list. If we have two pointers, `first` and `second`, and `first` is `n` steps ahead of `second`, then when `first` reaches the end of the list, `second` will be at the node *just before* the `n`th node from the end.

1.  **Dummy Node:** We create a `dummy` node and point its `next` to the `head` of the original list. This simplifies handling edge cases, especially when the node to be removed is the head of the original list.

2.  **Initialize Pointers:** Both `first` and `second` pointers are initialized to the `dummy` node.

3.  **Advance `first`:** We move the `first` pointer `n + 1` steps ahead. This creates a gap of `n` nodes between `first` and `second`.

4.  **Move Both Pointers:** We then move both `first` and `second` pointers one step at a time until `first` reaches `None` (the end of the list).
    -   When `first` becomes `None`, `second` will be pointing to the node *before* the `n`th node from the end.

5.  **Remove Node:** We then update `second.next` to `second.next.next`, effectively bypassing and removing the `n`th node from the end.

6.  **Return Result:** We return `dummy.next`, which is the head of the modified list.

**Time and Space Complexity:**

-   **Time Complexity:** O(L), where L is the length of the linked list. We traverse the list at most twice (once to advance `first`, and then both pointers move together).
-   **Space Complexity:** O(1), as we only use a few extra pointers.
