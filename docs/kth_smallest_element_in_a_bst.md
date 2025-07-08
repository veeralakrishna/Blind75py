
# 64. Kth Smallest Element in a BST

## Problem Statement

Given the `root` of a Binary Search Tree (BST) and an integer `k`, return *the `k`th smallest value (1-indexed) of all the values of the nodes in the tree*.

**Example 1:**

![Kth Smallest Element in a BST Example 1](https://assets.leetcode.com/uploads/2021/02/19/kthtree.jpg)

**Input:** root = [3,1,4,null,2], k = 1
**Output:** 1

**Example 2:**

![Kth Smallest Element in a BST Example 2](https://assets.leetcode.com/uploads/2021/02/19/kthtree2.jpg)

**Input:** root = [5,3,6,2,4,null,null,1], k = 3
**Output:** 3

## Solution

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def kthSmallest(self, root: TreeNode, k: int) -> int:
        stack = []
        current = root

        while current or stack:
            # Traverse to the leftmost node, pushing nodes onto the stack
            while current:
                stack.append(current)
                current = current.left

            # Pop the smallest node (leftmost node in the current subtree)
            current = stack.pop()
            k -= 1

            # If k becomes 0, this is our kth smallest element
            if k == 0:
                return current.val

            # Move to the right subtree to find the next smallest element
            current = current.right
```

## Explanation

The property of a Binary Search Tree (BST) is that an in-order traversal of the tree yields the nodes in ascending order. Therefore, to find the `k`th smallest element, we can perform an in-order traversal and stop when we have visited `k` nodes.

**Iterative In-order Traversal (using a stack):**

1.  **Initialization:**
    -   Create an empty `stack`.
    -   Initialize `current` to `root`.

2.  **Traverse Left:** While `current` is not `None`:
    -   Push `current` onto the `stack`.
    -   Move `current` to `current.left`. This ensures we go as far left as possible, finding the smallest elements first.

3.  **Process Node:** Once `current` is `None` (meaning we've reached the leftmost node of the current subtree):
    -   Pop a node from the `stack`. This is the next smallest element in the in-order traversal.
    -   Decrement `k`.
    -   If `k` becomes 0, we have found our `k`th smallest element. Return its `val`.

4.  **Traverse Right:** Move `current` to the `right` child of the popped node. This prepares for finding the next smallest elements in the right subtree.

This process continues until the `k`th smallest element is found.

**Time and Space Complexity:**

-   **Time Complexity:** O(H + k), where H is the height of the BST. In the worst case (a skewed tree), H can be N (number of nodes), so O(N). In a balanced BST, H is log N, so O(log N + k). We visit at most `k` nodes and traverse down to the `k`th node.
-   **Space Complexity:** O(H) for the stack. In the worst case, O(N), and in a balanced tree, O(log N).
