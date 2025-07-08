
# 60. Subtree of Another Tree

## Problem Statement

Given the roots of two binary trees `root` and `subRoot`, return `true` if there is a subtree of `root` with the same structure and node values of `subRoot` and `false` otherwise.

A subtree of a binary tree `tree` is a tree that consists of a node in `tree` and all of this node's descendants. The tree `tree` could also be considered as a subtree of itself.

**Example 1:**

![Subtree of Another Tree Example 1](https://assets.leetcode.com/uploads/2020/08/14/sample_1_leetcode_572.png)

**Input:** root = [3,4,5,1,2], subRoot = [4,1,2]
**Output:** true

**Example 2:**

![Subtree of Another Tree Example 2](https://assets.leetcode.com/uploads/2020/08/14/sample_2_leetcode_572.png)

**Input:** root = [3,4,5,1,2,null,null,null,null,0], subRoot = [4,1,2]
**Output:** false

## Solution

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def isSubtree(self, root: TreeNode, subRoot: TreeNode) -> bool:
        if not root:
            return False
        if not subRoot:
            return True # An empty tree is always a subtree

        # Check if the current root and subRoot are the same tree
        if self.isSameTree(root, subRoot):
            return True

        # Otherwise, check if subRoot is a subtree of root.left or root.right
        return self.isSubtree(root.left, subRoot) or self.isSubtree(root.right, subRoot)

    def isSameTree(self, p: TreeNode, q: TreeNode) -> bool:
        # Base Case 1: Both are None, they are the same
        if not p and not q:
            return True

        # Base Case 2: One is None and the other is not, they are different
        if not p or not q:
            return False

        # Base Case 3: Values are different, they are different
        if p.val != q.val:
            return False

        # Recursive Step: Check left subtrees and right subtrees
        return self.isSameTree(p.left, q.left) and self.isSameTree(p.right, q.right)
```

## Explanation

This problem combines two concepts: checking if two trees are identical and traversing a tree to find a match.

**Core Idea:** We need to check if `subRoot` is identical to `root`, or if `subRoot` is identical to any of `root`'s subtrees (left or right).

**Recursive Approach:**

1.  **`isSubtree(root, subRoot)` Function:**
    -   **Base Case 1 (`root` is `None`):** If the `root` of the main tree is `None`, then `subRoot` cannot be a subtree (unless `subRoot` is also `None`, which is handled by the next base case). So, return `False`.
    -   **Base Case 2 (`subRoot` is `None`):** If `subRoot` is `None`, an empty tree is considered a subtree of any tree. Return `True`.
    -   **Check for Identity:** First, we check if the current `root` and `subRoot` are exactly the same tree using a helper function `isSameTree`. If they are, we've found our subtree, so return `True`.
    -   **Recurse on Subtrees:** If they are not the same, we recursively check if `subRoot` is a subtree of `root.left` OR `root.right`. The `or` condition is important because `subRoot` could be found in either branch.

2.  **`isSameTree(p, q)` Helper Function:** This is a standard function to check if two binary trees are identical (same structure and same values). It has the following logic:
    -   If both `p` and `q` are `None`, return `True`.
    -   If one is `None` and the other is not, return `False`.
    -   If their values are different, return `False`.
    -   Otherwise, recursively check if their left children are the same AND their right children are the same.

**Time and Space Complexity:**

-   **Time Complexity:** O(M * N), where M is the number of nodes in `root` and N is the number of nodes in `subRoot`. In the worst case, for each node in `root`, we might perform a `isSameTree` comparison which takes O(N) time.
-   **Space Complexity:** O(H_root + H_subRoot) in the worst case, where H is the height of the respective trees, due to the recursion stack.
