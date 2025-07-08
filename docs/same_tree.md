
# 59. Same Tree

## Problem Statement

Given the roots of two binary trees `p` and `q`, return `true` if they are the same tree, and `false` otherwise.

Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.

**Example 1:**

![Same Tree Example 1](https://assets.leetcode.com/uploads/2020/12/12/samentree.jpg)

**Input:** p = [1,2,3], q = [1,2,3]
**Output:** true

**Example 2:**

![Same Tree Example 2](https://assets.leetcode.com/uploads/2020/12/12/samentree2.jpg)

**Input:** p = [1,2], q = [1,null,2]
**Output:** false

**Example 3:**

![Same Tree Example 3](https://assets.leetcode.com/uploads/2020/12/12/samentree3.jpg)

**Input:** p = [1,2,1], q = [1,1,2]
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

This problem can be solved using a recursive approach, comparing the two trees node by node.

**Core Idea:** Two binary trees are considered the same if:

1.  Both are empty (`None`).
2.  Both are non-empty, their root nodes have the same value, AND their left subtrees are the same, AND their right subtrees are the same.

**Recursive Approach:**

1.  **Base Case 1 (Both `None`):** If both `p` and `q` are `None`, it means we've reached the end of a branch in both trees simultaneously, and they are structurally identical at this point. Return `True`.

2.  **Base Case 2 (One `None`, One Not):** If one of `p` or `q` is `None` but the other is not, it means they are structurally different (one has a node where the other doesn't). Return `False`.

3.  **Base Case 3 (Different Values):** If both `p` and `q` are not `None`, but their `val` attributes are different, then the trees are not the same. Return `False`.

4.  **Recursive Step:** If none of the above base cases are met, it means `p` and `q` are both non-`None` and have the same value. Now, we need to recursively check if their left subtrees are the same AND if their right subtrees are the same. We use the logical `and` operator because both conditions must be true for the trees to be identical.

**Time and Space Complexity:**

-   **Time Complexity:** O(N), where N is the minimum number of nodes in the two trees. We visit each node at most once.
-   **Space Complexity:** O(H) in the worst case, where H is the height of the tree, due to the recursion stack. In a skewed tree, H can be N (O(N)), and in a balanced tree, H is log N (O(log N)).
