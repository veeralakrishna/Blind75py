
# 55. Remove Duplicates from Sorted List II

## Problem Statement

Given the `head` of a sorted linked list, *delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list*. Return *the linked list sorted as well*.

**Example 1:**

![Remove Duplicates from Sorted List II Example 1](https://assets.leetcode.com/uploads/2021/02/19/remval1.jpg)

**Input:** head = [1,2,3,3,4,4,5]
**Output:** [1,2,5]

**Example 2:**

![Remove Duplicates from Sorted List II Example 2](https://assets.leetcode.com/uploads/2021/02/19/remval2.jpg)

**Input:** head = [1,1,1,2,3]
**Output:** [2,3]

## Solution

```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        # Create a dummy node to handle cases where the head might be a duplicate
        dummy = ListNode(0, head)
        prev = dummy

        while head:
            # If it's a duplicate (current node's value is same as next node's value)
            # and next node exists
            if head.next and head.val == head.next.val:
                # Skip all nodes with the same value
                while head.next and head.val == head.next.val:
                    head = head.next
                # Link prev to the node after the duplicates
                prev.next = head.next
            else:
                # Not a duplicate, move prev forward
                prev = prev.next
            # Move head forward
            head = head.next

        return dummy.next
```

## Explanation

This problem is a more complex version of "Remove Duplicates from Sorted List". Here, we need to remove *all* nodes that have duplicate numbers, not just the duplicate occurrences themselves.

**Algorithm:**

1.  **Dummy Node:** We use a `dummy` node to simplify handling cases where the original `head` itself might be a duplicate and needs to be removed. `prev` pointer is initialized to `dummy`.

2.  **Iterate with `head` and `prev`:** We use `head` to traverse the list and `prev` to build the new list without duplicates.

3.  **Duplicate Detection and Removal:**
    -   **Condition for Duplicate:** We check if `head.next` exists and if `head.val == head.next.val`. This indicates that `head` is part of a sequence of duplicates.
    -   **Skip Duplicates:** If a duplicate sequence is found, we enter an inner `while` loop. This loop advances `head` past all nodes that have the same value as the current `head.val`.
    -   **Relink `prev`:** After the inner loop, `head` will be pointing to the last duplicate node (or the node just before the next distinct value). We then update `prev.next = head.next`. This effectively removes the entire block of duplicate nodes from the list.

4.  **Non-Duplicate Case:** If `head.next` does not exist or `head.val != head.next.val`, it means `head` is a unique node (or the last node in a sequence of duplicates). In this case, we simply advance `prev` to `prev.next`.

5.  **Advance `head`:** In both cases (duplicate or not), we advance `head` to `head.next` to continue scanning the list.

6.  **Return Result:** `dummy.next` will be the head of the modified list.

**Time and Space Complexity:**

-   **Time Complexity:** O(n), where n is the number of nodes in the linked list. We iterate through the list once.
-   **Space Complexity:** O(1), as we only use a few extra pointers.
