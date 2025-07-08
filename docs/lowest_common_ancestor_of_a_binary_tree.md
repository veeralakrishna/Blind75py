
# 68. Lowest Common Ancestor of a Binary Tree

## Problem Statement

Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the definition of LCA on Wikipedia: "The lowest common ancestor is defined between two nodes `p` and `q` as the lowest node in `T` that has both `p` and `q` as descendants (where we allow a node to be a descendant of itself)."

**Example 1:**

![Lowest Common Ancestor of a Binary Tree Example 1](https://assets.leetcode.com/uploads/2018/12/14/binarytree_lca_example1.png)

**Input:** root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
**Output:** 3
**Explanation:** The LCA of nodes 5 and 1 is 3.

**Example 2:**

![Lowest Common Ancestor of a Binary Tree Example 2](https://assets.leetcode.com/uploads/2018/12/14/binarytree_lca_example2.png)

**Input:** root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
**Output:** 5
**Explanation:** The LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.

**Example 3:**

**Input:** root = [1,2], p = 1, q = 2
**Output:** 1

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
        # Base Case 1: If root is None, or root is p or q, then root is the LCA
        if not root or root == p or root == q:
            return root

        # Recursively search in left and right subtrees
        left_lca = self.lowestCommonAncestor(root.left, p, q)
        right_lca = self.lowestCommonAncestor(root.right, p, q)

        # Case 1: Both p and q are found in different subtrees
        # This means the current root is the LCA
        if left_lca and right_lca:
            return root
        # Case 2: Only one of p or q is found (or both are in the same subtree)
        # If left_lca is not None, it means p or q (or both) were found in the left subtree.
        # So, left_lca is the LCA.
        elif left_lca:
            return left_lca
        # Case 3: Only right_lca is not None, meaning p or q (or both) were found in the right subtree.
        # So, right_lca is the LCA.
        else:
            return right_lca
```

## Explanation

This problem is a classic recursive tree traversal problem. Unlike a BST, in a general binary tree, we cannot use the node values to guide our search. We must explore both left and right subtrees.

**Core Idea:** The lowest common ancestor (LCA) of two nodes `p` and `q` can be found by recursively searching the tree. The LCA is the first node where `p` and `q` are found in different subtrees, or where one of them *is* the current node and the other is in one of its subtrees.

**Recursive Approach:**

1.  **Base Case:**
    -   If `root` is `None`, it means we've reached the end of a branch without finding `p` or `q`. Return `None`.
    -   If `root` is `p` or `root` is `q`, it means we've found one of the target nodes. According to the definition, a node can be a descendant of itself, so this `root` is a potential LCA. Return `root`.

2.  **Recursive Calls:**
    -   Recursively call `lowestCommonAncestor` on the `root.left` subtree to find `left_lca`.
    -   Recursively call `lowestCommonAncestor` on the `root.right` subtree to find `right_lca`.

3.  **Combine Results:** After the recursive calls return:
    -   **If both `left_lca` and `right_lca` are not `None`:** This means `p` was found in one subtree and `q` in the other (or vice versa). Therefore, the current `root` is the LCA. Return `root`.
    -   **If only `left_lca` is not `None`:** This means both `p` and `q` (or just one of them) were found in the left subtree. So, `left_lca` is the LCA. Return `left_lca`.
    -   **If only `right_lca` is not `None`:** This means both `p` and `q` (or just one of them) were found in the right subtree. So, `right_lca` is the LCA. Return `right_lca`.
    -   **If both are `None`:** Neither `p` nor `q` were found in the current subtree. Return `None`.

**Time and Space Complexity:**

-   **Time Complexity:** O(N), where N is the number of nodes in the binary tree. In the worst case, we visit each node once.
-   **Space Complexity:** O(H) for the recursion stack, where H is the height of the tree. In a skewed tree, H can be N (O(N)), and in a balanced tree, H is log N (O(log N)).
