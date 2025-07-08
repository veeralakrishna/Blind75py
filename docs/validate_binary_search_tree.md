
# 63. Validate Binary Search Tree

## Problem Statement

Given the `root` of a binary tree, determine if it is a valid Binary Search Tree (BST).

A **valid BST** is defined as follows:

- The left subtree of a node contains only nodes with keys **less than** the node's key.
- The right subtree of a node contains only nodes with keys **greater than** the node's key.
- Both the left and right subtrees must also be binary search trees.

**Example 1:**

![Validate Binary Search Tree Example 1](https://assets.leetcode.com/uploads/2020/12/09/tree1.jpg)

**Input:** root = [2,1,3]
**Output:** true

**Example 2:**

![Validate Binary Search Tree Example 2](https://assets.leetcode.com/uploads/2020/12/09/tree2.jpg)

**Input:** root = [5,1,4,null,null,3,6]
**Output:** false
**Explanation:** The root node's value is 5 but its right child's value is 4.

## Solution

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def isValidBST(self, root: TreeNode) -> bool:
        # Helper function with min_val and max_val bounds
        def validate(node, min_val, max_val):
            if not node:
                return True

            # Check if the current node's value is within the valid range
            if not (min_val < node.val < max_val):
                return False

            # Recursively validate left and right subtrees
            # For left subtree, max_val becomes node.val
            # For right subtree, min_val becomes node.val
            return (validate(node.left, min_val, node.val) and
                    validate(node.right, node.val, max_val))

        # Start validation with initial bounds of negative and positive infinity
        return validate(root, float('-inf'), float('inf'))
```

## Explanation

To validate a Binary Search Tree (BST), it's not enough to just check if a node's left child is smaller and its right child is larger. We need to ensure that *all* nodes in the left subtree are smaller than the current node, and *all* nodes in the right subtree are larger than the current node.

**Core Idea:** We can use a recursive approach that passes down a valid range (`min_val`, `max_val`) for each node. A node's value must be strictly within this range.

**Recursive `validate` Function:**

-   The `validate` function takes a `node`, a `min_val`, and a `max_val` as arguments.

1.  **Base Case:** If `node` is `None`, it's a valid empty subtree, so return `True`.

2.  **Range Check:** Check if `node.val` is strictly greater than `min_val` AND strictly less than `max_val`. If not, the BST property is violated, so return `False`.

3.  **Recursive Calls:**
    -   **Left Subtree:** Recursively call `validate` on `node.left`. The `min_val` for the left subtree remains the same, but the `max_val` becomes `node.val` (because all nodes in the left subtree must be less than the current node).
    -   **Right Subtree:** Recursively call `validate` on `node.right`. The `max_val` for the right subtree remains the same, but the `min_val` becomes `node.val` (because all nodes in the right subtree must be greater than the current node).

4.  **Combine Results:** The current node is valid only if both its left and right subtrees are also valid BSTs. So, we return the logical `and` of the two recursive calls.

**Initial Call:** We start the validation from the `root` with `min_val` as negative infinity and `max_val` as positive infinity.

**Time and Space Complexity:**

-   **Time Complexity:** O(N), where N is the number of nodes in the binary tree. Each node is visited exactly once.
-   **Space Complexity:** O(H) in the worst case, where H is the height of the tree, due to the recursion stack. In a skewed tree, H can be N (O(N)), and in a balanced tree, H is log N (O(log N)).
