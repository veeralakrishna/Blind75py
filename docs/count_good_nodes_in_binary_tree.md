
# 67. Count Good Nodes in Binary Tree

## Problem Statement

Given a binary tree `root`, a node `X` in the tree is named **good** if in the path from the root to `X` there are no nodes with a value greater than `X`.

Return *the number of **good** nodes in the binary tree*.

**Example 1:**

![Count Good Nodes in Binary Tree Example 1](https://assets.leetcode.com/uploads/2020/07/14/good_nodes_example_1.png)

**Input:** root = [3,1,4,3,null,1,5]
**Output:** 4
**Explanation:** Nodes in the tree are:
- Node 3 (root) is always a good node.
- Node 1 -> not good, because path from root is [3,1], and 3 > 1.
- Node 4 -> good, because path from root is [3,4], and 3 < 4.
- Node 3 (left child of 1) -> not good, because path from root is [3,1,3], and 3 > 1.
- Node 1 (left child of 4) -> not good, because path from root is [3,4,1], and 4 > 1.
- Node 5 (right child of 4) -> good, because path from root is [3,4,5], and 3 < 5, 4 < 5.
Total good nodes: 4.

**Example 2:**

**Input:** root = [3,3,null,4,2]
**Output:** 3
**Explanation:** Node 3 (root) is good.
Node 3 (left child of 3) is good because path is [3,3].
Node 4 (left child of 3) is good because path is [3,3,4].
Node 2 (right child of 4) is not good because path is [3,3,4,2], and 4 > 2.

**Example 3:**

**Input:** root = [1]
**Output:** 1

## Solution

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def goodNodes(self, root: TreeNode) -> int:
        def dfs(node, max_so_far):
            if not node:
                return 0

            count = 0
            if node.val >= max_so_far:
                count = 1
                max_so_far = node.val # Update max_so_far for children

            count += dfs(node.left, max_so_far)
            count += dfs(node.right, max_so_far)

            return count

        # Start DFS from root with initial max_so_far as negative infinity
        return dfs(root, float('-inf'))
```

## Explanation

This problem can be solved using a Depth-First Search (DFS) approach. The key is to pass the maximum value encountered so far along the path from the root to the current node.

**Core Idea:** A node is "good" if its value is greater than or equal to the maximum value on the path from the root to its parent.

**Recursive `dfs` Function:**

-   The `dfs` function takes two arguments: the `node` currently being visited and `max_so_far`, which is the maximum value encountered on the path from the root to the parent of the current `node`.

1.  **Base Case:** If `node` is `None`, there are no good nodes in this empty subtree, so return 0.

2.  **Check Good Node:**
    -   Initialize `count` for the current node to 0.
    -   If `node.val >= max_so_far`, it means the current node is a "good" node. Increment `count` to 1.
    -   Crucially, update `max_so_far` to `node.val` if the current node is greater. This new `max_so_far` will be passed down to its children.

3.  **Recurse on Children:**
    -   Recursively call `dfs` on `node.left` and `node.right`, passing the (potentially updated) `max_so_far`.
    -   Add the counts returned from the recursive calls to the current `count`.

4.  **Return Count:** Return the total `count` of good nodes in the subtree rooted at `node`.

**Initial Call:** We start the DFS from the `root` with `max_so_far` initialized to negative infinity (`float('-inf')`) to ensure the root node is always considered a good node.

**Time and Space Complexity:**

-   **Time Complexity:** O(N), where N is the number of nodes in the binary tree. Each node is visited exactly once.
-   **Space Complexity:** O(H) in the worst case, where H is the height of the tree, due to the recursion stack. In a skewed tree, H can be N (O(N)), and in a balanced tree, H is log N (O(log N)).
