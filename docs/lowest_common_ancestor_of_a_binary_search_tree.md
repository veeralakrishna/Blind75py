
# 61. Lowest Common Ancestor of a Binary Search Tree

## Problem Statement

Given a Binary Search Tree (BST), find the lowest common ancestor (LCA) of two given nodes in the BST.

According to the definition of LCA on Wikipedia: "The lowest common ancestor is defined between two nodes `p` and `q` as the lowest node in `T` that has both `p` and `q` as descendants (where we allow a node to be a descendant of itself)."

**Example 1:**

![Lowest Common Ancestor of a Binary Search Tree Example 1](https://assets.leetcode.com/uploads/2018/12/14/binarysearchtree_lca_example1.png)

**Input:** root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
**Output:** 6
**Explanation:** The LCA of nodes 2 and 8 is 6.

**Example 2:**

![Lowest Common Ancestor of a Binary Search Tree Example 2](https://assets.leetcode.com/uploads/2018/12/14/binarysearchtree_lca_example2.png)

**Input:** root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
**Output:** 2
**Explanation:** The LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.

**Example 3:**

**Input:** root = [2,1], p = 2, q = 1
**Output:** 2

## Solution

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        # If both p and q are smaller than the current root, then LCA must be in the left subtree
        if p.val < root.val and q.val < root.val:
            return self.lowestCommonAncestor(root.left, p, q)
        # If both p and q are larger than the current root, then LCA must be in the right subtree
        elif p.val > root.val and q.val > root.val:
            return self.lowestCommonAncestor(root.right, p, q)
        # If one is smaller and the other is larger, or one of them is the root itself,
        # then the current root is the LCA.
        else:
            return root
```

## Explanation

This problem leverages the special property of a Binary Search Tree (BST): for any node, all values in its left subtree are smaller, and all values in its right subtree are larger.

**Core Idea:** The lowest common ancestor (LCA) of two nodes `p` and `q` in a BST will be the first node encountered during a traversal from the root where `p` and `q` are on opposite sides (one in the left subtree, one in the right subtree), or where one of `p` or `q` *is* the current node.

**Recursive Approach:**

1.  **Both `p` and `q` are smaller than `root.val`:** If both `p.val` and `q.val` are less than the current `root.val`, it means both `p` and `q` must reside in the left subtree of the current `root`. Therefore, the LCA must also be in the left subtree, so we recursively call `lowestCommonAncestor` on `root.left`.

2.  **Both `p` and `q` are larger than `root.val`:** Similarly, if both `p.val` and `q.val` are greater than the current `root.val`, they must both reside in the right subtree. We recursively call `lowestCommonAncestor` on `root.right`.

3.  **One is smaller, one is larger, or one is the `root` itself:** If neither of the above conditions is met, it means one of the following is true:
    -   `p.val < root.val` and `q.val > root.val` (or vice versa): `p` and `q` are on opposite sides of the current `root`. This means the current `root` is their LCA.
    -   `p.val == root.val` or `q.val == root.val`: One of the target nodes is the current `root`. According to the definition, a node can be a descendant of itself, so the current `root` is the LCA.
    In all these cases, the current `root` is the LCA, so we return `root`.

**Time and Space Complexity:**

-   **Time Complexity:** O(H), where H is the height of the BST. In the worst case (a skewed tree), H can be N (number of nodes), leading to O(N). In a balanced BST, H is log N, leading to O(log N).
-   **Space Complexity:** O(H) for the recursion stack. In the worst case, O(N), and in a balanced tree, O(log N).
