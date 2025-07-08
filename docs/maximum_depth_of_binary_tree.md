
# 57. Maximum Depth of Binary Tree

## Problem Statement

Given the `root` of a binary tree, return *its maximum depth*.

A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.

**Example 1:**

![Maximum Depth of Binary Tree Example 1](https://assets.leetcode.com/uploads/2020/08/14/depth1.jpg)

**Input:** root = [3,9,20,null,null,15,7]
**Output:** 3

**Example 2:**

**Input:** root = [1,null,2]
**Output:** 2

**Example 3:**

**Input:** root = []
**Output:** 0

## Solution

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def maxDepth(self, root: TreeNode) -> int:
        if not root:
            return 0

        # The maximum depth is 1 (for the current node) plus the maximum
        # depth of its left or right subtree.
        return 1 + max(self.maxDepth(root.left), self.maxDepth(root.right))
```

## Explanation

This problem can be solved very concisely using recursion.

**Core Idea:** The maximum depth of a binary tree is 1 (for the root node itself) plus the maximum depth of its left or right subtree.

**Recursive Approach:**

1.  **Base Case:** If the `root` is `None` (meaning we've reached past a leaf node), the depth is 0. This is our stopping condition for the recursion.

2.  **Recursive Step:**
    -   Recursively call `maxDepth` on the `root.left` subtree to find its maximum depth.
    -   Recursively call `maxDepth` on the `root.right` subtree to find its maximum depth.
    -   Take the maximum of these two depths.
    -   Add 1 to this maximum (to account for the current `root` node).

**Example Trace:**

For `root = [3,9,20,null,null,15,7]`:

-   `maxDepth(3)`:
    -   `1 + max(maxDepth(9), maxDepth(20))`
-   `maxDepth(9)`:
    -   `1 + max(maxDepth(None), maxDepth(None))`
    -   `1 + max(0, 0) = 1`
-   `maxDepth(20)`:
    -   `1 + max(maxDepth(15), maxDepth(7))`
-   `maxDepth(15)`:
    -   `1 + max(maxDepth(None), maxDepth(None)) = 1`
-   `maxDepth(7)`:
    -   `1 + max(maxDepth(None), maxDepth(None)) = 1`
-   So, `maxDepth(20)` becomes `1 + max(1, 1) = 2`.
-   Finally, `maxDepth(3)` becomes `1 + max(1, 2) = 3`.

**Time and Space Complexity:**

-   **Time Complexity:** O(N), where N is the number of nodes in the binary tree. We visit each node exactly once.
-   **Space Complexity:** O(H) in the worst case, where H is the height of the tree. This is due to the recursion stack. In a skewed tree, H can be N (O(N)), and in a balanced tree, H is log N (O(log N)).
