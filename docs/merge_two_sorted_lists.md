
# 43. Merge Two Sorted Lists

## Problem Statement

You are given the heads of two sorted linked lists `list1` and `list2`.

Merge the two lists into one **sorted** list. The list should be made by splicing together the nodes of the first two lists.

Return *the head of the merged linked list*.

**Example 1:**

![Merge Two Sorted Lists Example 1](https://assets.leetcode.com/uploads/2020/10/03/merge_linked_list_example_1.png)

**Input:** list1 = [1,2,4], list2 = [1,3,4]
**Output:** [1,1,2,3,4,4]

**Example 2:**

**Input:** list1 = [], list2 = []
**Output:** []

**Example 3:**

**Input:** list1 = [], list2 = [0]
**Output:** [0]

## Solution

```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def mergeTwoLists(self, list1: ListNode, list2: ListNode) -> ListNode:
        # Create a dummy node to simplify handling the head of the merged list
        dummy = ListNode()
        current = dummy

        while list1 and list2:
            if list1.val < list2.val:
                current.next = list1
                list1 = list1.next
            else:
                current.next = list2
                list2 = list2.next
            current = current.next

        # Attach the remaining part of the non-empty list
        if list1:
            current.next = list1
        elif list2:
            current.next = list2

        return dummy.next
```

## Explanation

This problem can be solved iteratively or recursively. The iterative approach is generally preferred for linked list problems to avoid potential recursion depth limits.

**Iterative Approach:**

1.  **Dummy Node:** We create a `dummy` node. This is a common technique in linked list problems to simplify the logic of handling the head of the new list. We also use a `current` pointer, initially pointing to `dummy`.

2.  **Traverse Both Lists:** We use a `while` loop that continues as long as both `list1` and `list2` have nodes.
    -   In each iteration, we compare the values of the current nodes in `list1` and `list2`.
    -   We append the node with the smaller value to `current.next`.
    -   We then advance the pointer of the list from which we took the node (either `list1` or `list2`).
    -   Finally, we advance the `current` pointer to the newly appended node.

3.  **Attach Remaining Nodes:** After the loop, one of the lists might still have remaining nodes (if one list was longer than the other). Since both input lists are sorted, these remaining nodes are already in sorted order. We simply attach the rest of the non-empty list to `current.next`.

4.  **Return Result:** The merged sorted list starts from `dummy.next`.

**Time and Space Complexity:**

-   **Time Complexity:** O(m + n), where m and n are the lengths of `list1` and `list2` respectively. We traverse each list at most once.
-   **Space Complexity:** O(1) because we are only using a few extra pointers and not creating new nodes (we are just rearranging existing nodes).
