
# 66. Binary Tree Right Side View

## Problem Statement

Given the `root` of a binary tree, imagine yourself standing on the **right side** of it, return *the values of the nodes you can see ordered from top to bottom*.

**Example 1:**

![Binary Tree Right Side View Example 1](https://assets.leetcode.com/uploads/2021/02/14/tree.jpg)

**Input:** root = [1,2,3,null,5,null,4]
**Output:** [1,3,4]

**Example 2:**

**Input:** root = [1,null,3]
**Output:** [1,3]

**Example 3:**

**Input:** root = []
**Output:** []

## Solution

```python
from collections import deque

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def rightSideView(self, root: TreeNode) -> list[int]:
        result = []
        if not root:
            return result

        queue = deque([root])

        while queue:
            level_size = len(queue)
            # The last node in the current level is the rightmost node
            rightmost_node_val = None

            for i in range(level_size):
                node = queue.popleft()
                rightmost_node_val = node.val # Update with current node's value

                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)

            result.append(rightmost_node_val)

        return result
```

## Explanation

This problem can be solved using a Level Order Traversal (BFS). The key insight is that for each level of the tree, the rightmost node is the last node processed in that level during a BFS traversal.

1.  **Initialization:**
    -   Create an empty list `result` to store the right side view.
    -   If the `root` is `None`, return an empty `result`.
    -   Initialize a `deque` (double-ended queue) and add the `root` to it.

2.  **Level-by-Level Traversal (BFS):**
    -   The `while queue:` loop continues as long as there are nodes to visit.
    -   Inside the loop, we first get the `level_size` (the number of nodes currently in the queue). This tells us how many nodes belong to the *current* level.
    -   We initialize `rightmost_node_val` to `None` for each level.
    -   Iterate `level_size` times:
        -   Dequeue a `node` from the front of the `queue`.
        -   Update `rightmost_node_val` with the `node.val`. Since we are processing nodes from left to right within a level, the last node processed will be the rightmost one.
        -   If the `node` has a `left` child, enqueue it.
        -   If the `node` has a `right` child, enqueue it.
    -   After processing all nodes at the current level, append the `rightmost_node_val` to the `result`.

3.  **Return Result:** Once the `queue` is empty, all nodes have been visited, and `result` contains the right side view.

**Time and Space Complexity:**

-   **Time Complexity:** O(N), where N is the number of nodes in the binary tree. Each node is visited and processed exactly once.
-   **Space Complexity:** O(W) in the worst case, where W is the maximum width of the tree (the maximum number of nodes at any single level). In a complete binary tree, W can be N/2, so O(N). In a skewed tree, W is 1, so O(1).
