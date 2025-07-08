
# 65. Construct Binary Tree from Preorder and Inorder Traversal

## Problem Statement

Given two integer arrays `preorder` and `inorder` where `preorder` is the preorder traversal of a binary tree and `inorder` is the inorder traversal of the same tree, construct and return *the binary tree*.

**Example 1:**

![Construct Binary Tree from Preorder and Inorder Traversal Example 1](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)

**Input:** preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
**Output:** [3,9,20,null,null,15,7]

**Example 2:**

**Input:** preorder = [-1], inorder = [-1]
**Output:** [-1]

## Solution

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def buildTree(self, preorder: list[int], inorder: list[int]) -> TreeNode:
        # Create a hash map for quick lookup of values in inorder traversal
        inorder_map = {val: idx for idx, val in enumerate(inorder)}

        def build(preorder_start, preorder_end, inorder_start, inorder_end):
            # Base case: if the range is invalid, return None
            if preorder_start > preorder_end or inorder_start > inorder_end:
                return None

            # The first element in preorder is always the root of the current subtree
            root_val = preorder[preorder_start]
            root = TreeNode(root_val)

            # Find the root_val in inorder traversal to determine left and right subtree boundaries
            inorder_root_idx = inorder_map[root_val]

            # Calculate the size of the left subtree
            left_subtree_size = inorder_root_idx - inorder_start

            # Recursively build the left subtree
            root.left = build(
                preorder_start + 1,              # Next element in preorder is left child
                preorder_start + left_subtree_size, # End of left subtree in preorder
                inorder_start,                   # Start of left subtree in inorder
                inorder_root_idx - 1             # End of left subtree in inorder
            )

            # Recursively build the right subtree
            root.right = build(
                preorder_start + left_subtree_size + 1, # Start of right subtree in preorder
                preorder_end,                       # End of right subtree in preorder
                inorder_root_idx + 1,               # Start of right subtree in inorder
                inorder_end                         # End of right subtree in inorder
            )

            return root

        # Initial call to build the tree
        return build(0, len(preorder) - 1, 0, len(inorder) - 1)
```

## Explanation

This is a classic tree construction problem that can be solved recursively. The key properties of preorder and inorder traversals are crucial here:

-   **Preorder Traversal:** `[Root, Left Subtree, Right Subtree]`
-   **Inorder Traversal:** `[Left Subtree, Root, Right Subtree]`

**Core Idea:**

1.  The first element in the `preorder` traversal is always the root of the current subtree.
2.  Once we identify the root from `preorder`, we can find its position in the `inorder` traversal. This position divides the `inorder` array into two parts: the left part represents the left subtree, and the right part represents the right subtree.
3.  The sizes of these left and right parts in `inorder` tell us the sizes of the left and right subtrees. We can then use these sizes to determine the corresponding ranges in the `preorder` array for the left and right subtrees.

**Recursive `build` Function:**

-   The `build` function takes the start and end indices for both `preorder` and `inorder` arrays, defining the current segment of the tree we are trying to build.

1.  **Base Case:** If `preorder_start > preorder_end` or `inorder_start > inorder_end`, it means we have an empty subtree, so return `None`.

2.  **Create Root:** The `root_val` is `preorder[preorder_start]`. Create a `TreeNode` with this value.

3.  **Find Root in Inorder:** Use a hash map (`inorder_map`) to quickly find the index of `root_val` in the `inorder` array. This `inorder_root_idx` is critical.

4.  **Calculate Subtree Sizes:** The number of elements in the left subtree is `left_subtree_size = inorder_root_idx - inorder_start`.

5.  **Recursive Calls:**
    -   **Left Subtree:** Recursively call `build` for the left subtree. The `preorder` range will be `[preorder_start + 1, preorder_start + left_subtree_size]`. The `inorder` range will be `[inorder_start, inorder_root_idx - 1]`.
    -   **Right Subtree:** Recursively call `build` for the right subtree. The `preorder` range will be `[preorder_start + left_subtree_size + 1, preorder_end]`. The `inorder` range will be `[inorder_root_idx + 1, inorder_end]`.

6.  **Return Root:** Return the constructed `root` node.

**Optimization:** Using a hash map for `inorder_map` allows for O(1) lookup of the root's index, which is crucial for performance.

**Time and Space Complexity:**

-   **Time Complexity:** O(N), where N is the number of nodes in the tree. Each node is processed once, and the hash map lookup is O(1).
-   **Space Complexity:** O(N) for the `inorder_map` and O(H) for the recursion stack (where H is the height of the tree, which can be N in the worst case).
