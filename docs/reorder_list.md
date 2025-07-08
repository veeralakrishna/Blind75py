
# 45. Reorder List

## Problem Statement

You are given the head of a singly linked list `L`: `L0 → L1 → ... → Ln-1 → Ln`.

Reorder the list to be on the following form: `L0 → Ln → L1 → Ln-1 → L2 → Ln-2 → ...`

You may not modify the values in the list's nodes. Only nodes themselves may be changed.

**Example 1:**

![Reorder List Example 1](https://assets.leetcode.com/uploads/2021/02/19/reorder1linked-list.jpg)

**Input:** head = [1,2,3,4]
**Output:** [1,4,2,3]

**Example 2:**

![Reorder List Example 2](https://assets.leetcode.com/uploads/2021/02/19/reorder2linked-list.jpg)

**Input:** head = [1,2,3,4,5]
**Output:** [1,5,2,4,3]

## Solution

```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def reorderList(self, head: ListNode) -> None:
        """
        Do not return anything, modify head in-place instead.
        """
        if not head or not head.next:
            return

        # Step 1: Find the middle of the linked list
        slow, fast = head, head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next

        # slow is now at the middle node
        # Step 2: Split the list into two halves
        # first_half: head -> ... -> slow (before split)
        # second_half: slow.next -> ... -> None
        second_half = slow.next
        slow.next = None  # Break the link to separate the two halves
        first_half = head

        # Step 3: Reverse the second half of the list
        prev = None
        curr = second_half
        while curr:
            next_temp = curr.next
            curr.next = prev
            prev = curr
            curr = next_temp
        second_half_reversed = prev

        # Step 4: Merge the two halves
        # L0 -> L1 -> L2 -> ...
        # Ln -> Ln-1 -> Ln-2 -> ...
        # Merge: L0 -> Ln -> L1 -> Ln-1 -> ...
        p1, p2 = first_half, second_half_reversed
        while p2:
            temp1 = p1.next
            temp2 = p2.next

            p1.next = p2
            p2.next = temp1

            p1 = temp1
            p2 = temp2
```

## Explanation

This problem requires reordering a linked list in a specific pattern. It can be broken down into three main steps:

1.  **Find the Middle of the Linked List:**
    -   We use the fast and slow pointer technique. The `fast` pointer moves two steps at a time, and the `slow` pointer moves one step at a time. When `fast` reaches the end (or `None`), `slow` will be at the middle of the list.

2.  **Split the List into Two Halves:**
    -   Once `slow` is at the middle, we break the link between the first half and the second half. `slow.next` becomes `None`, effectively terminating the first half.
    -   The second half starts from `slow.next` (before `slow.next` was set to `None`).

3.  **Reverse the Second Half:**
    -   We reverse the second half of the linked list. This is a standard linked list reversal operation (as seen in problem 44, "Reverse Linked List").

4.  **Merge the Two Halves:**
    -   Now we have two lists: the first half (`L0 -> L1 -> L2 -> ...`) and the reversed second half (`Ln -> Ln-1 -> Ln-2 -> ...`).
    -   We merge them by alternating nodes: `L0` points to `Ln`, `Ln` points to `L1`, `L1` points to `Ln-1`, and so on.
    -   We use two pointers, `p1` for the first half and `p2` for the reversed second half. In each step, we adjust the `next` pointers to interleave the nodes.

**Time and Space Complexity:**

-   **Time Complexity:** O(n), where n is the number of nodes in the linked list. Each step (finding middle, splitting, reversing, merging) takes linear time.
-   **Space Complexity:** O(1), as we are modifying the list in-place and only using a few extra pointers.
