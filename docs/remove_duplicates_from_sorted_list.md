
# 54. Remove Duplicates from Sorted List

## Problem Statement

Given the `head` of a sorted linked list, *delete all duplicates such that each element appears only once*. Return *the linked list sorted as well*.

**Example 1:**

![Remove Duplicates from Sorted List Example 1](https://assets.leetcode.com/uploads/2021/02/19/remval1.jpg)

**Input:** head = [1,1,2]
**Output:** [1,2]

**Example 2:**

![Remove Duplicates from Sorted List Example 2](https://assets.leetcode.com/uploads/2021/02/19/remval2.jpg)

**Input:** head = [1,1,2,3,3]
**Output:** [1,2,3]

## Solution

```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        current = head

        while current and current.next:
            if current.val == current.next.val:
                # Skip the duplicate node
                current.next = current.next.next
            else:
                # Move to the next node if no duplicate is found
                current = current.next

        return head
```

## Explanation

This problem is straightforward because the linked list is already sorted. This means any duplicate values will be adjacent to each other.

**Algorithm:**

1.  **Initialize `current` pointer:** Start a `current` pointer at the `head` of the linked list.

2.  **Iterate and Compare:** Iterate through the list as long as `current` and `current.next` are not `None`.
    -   **If `current.val` is equal to `current.next.val`:** This indicates a duplicate. To remove the duplicate, we simply bypass `current.next` by setting `current.next = current.next.next`. We do *not* advance `current` in this case, because the new `current.next` might also be a duplicate of `current.val`.
    -   **If `current.val` is not equal to `current.next.val`:** No duplicate is found, so we advance `current` to `current.next` to check the next pair.

3.  **Return Head:** After the loop finishes, the list will have all duplicates removed, and we return the original `head` (which might have changed if the first few elements were duplicates).

**Time and Space Complexity:**

-   **Time Complexity:** O(n), where n is the number of nodes in the linked list, as we iterate through the list once.
-   **Space Complexity:** O(1), as we only use a few extra pointers.
