
# 48. Swap Nodes in Pairs

## Problem Statement

Given a `head` of a linked list, swap every two adjacent nodes and return its head.

You must solve the problem without modifying the values in the list's nodes (i.e., only changes to the nodes themselves are allowed).

**Example 1:**

![Swap Nodes in Pairs Example 1](https://assets.leetcode.com/uploads/2020/10/03/swap_nodes_example_1.png)

**Input:** head = [1,2,3,4]
**Output:** [2,1,4,3]

**Example 2:**

**Input:** head = []
**Output:** []

**Example 3:**

**Input:** head = [1]
**Output:** [1]

## Solution

```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def swapPairs(self, head: ListNode) -> ListNode:
        # Create a dummy node to handle edge cases (e.g., swapping the head)
        dummy = ListNode(0)
        dummy.next = head
        prev = dummy

        while prev.next and prev.next.next:
            # Nodes to be swapped
            first = prev.next
            second = prev.next.next

            # Swapping logic
            first.next = second.next
            second.next = first
            prev.next = second

            # Move prev pointer for next iteration
            prev = first

        return dummy.next
```

## Explanation

This problem involves modifying linked list pointers to swap adjacent nodes. An iterative approach is generally preferred.

**Core Idea:** We need to keep track of the node *before* the pair we are about to swap, as its `next` pointer will need to be updated to point to the second node of the swapped pair.

1.  **Dummy Node:** We use a `dummy` node to simplify handling the head of the list, especially if the original head needs to be swapped.

2.  **`prev` Pointer:** We initialize a `prev` pointer to the `dummy` node. This `prev` pointer will always point to the node *before* the current pair we are considering for swapping.

3.  **Iteration:** We iterate as long as there is a pair to swap (`prev.next` and `prev.next.next` are not `None`).

4.  **Swapping Logic:** Inside the loop:
    -   `first` points to the first node of the pair (`prev.next`).
    -   `second` points to the second node of the pair (`prev.next.next`).
    -   **`first.next = second.next`:** The `first` node (which will become the second in the swapped pair) should now point to the node that was originally after `second`.
    -   **`second.next = first`:** The `second` node (which will become the first in the swapped pair) should now point to the `first` node.
    -   **`prev.next = second`:** The node before the pair (`prev`) should now point to the `second` node (which is now the new first node of the swapped pair).

5.  **Advance `prev`:** After swapping, `prev` is moved to `first` (the original `first` node, which is now the second node of the swapped pair) to prepare for the next pair.

6.  **Return Result:** `dummy.next` will be the head of the modified list.

**Time and Space Complexity:**

-   **Time Complexity:** O(n), where n is the number of nodes in the linked list, as we iterate through the list once.
-   **Space Complexity:** O(1), as we only use a few extra pointers.
