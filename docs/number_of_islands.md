
# 30. Number of Islands

## Problem Statement

Given an `m x n` 2D binary grid `grid` which represents a map of '1's (land) and '0's (water), return *the number of islands*.

An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example 1:**

**Input:** grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
**Output:** 1

**Example 2:**

**Input:** grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
**Output:** 3

## Solution

```python
class Solution:
    def numIslands(self, grid: list[list[str]]) -> int:
        if not grid or not grid[0]:
            return 0

        rows, cols = len(grid), len(grid[0])
        num_islands = 0

        def dfs(r, c):
            # Base cases for DFS:
            # 1. Out of bounds
            # 2. Current cell is water ('0')
            # 3. Current cell has already been visited (marked as '2')
            if (
                r < 0 or r >= rows or c < 0 or c >= cols
                or grid[r][c] == '0' or grid[r][c] == '2'
            ):
                return

            # Mark the current cell as visited
            grid[r][c] = '2'

            # Explore neighbors
            dfs(r + 1, c)
            dfs(r - 1, c)
            dfs(r, c + 1)
            dfs(r, c - 1)

        for r in range(rows):
            for c in range(cols):
                if grid[r][c] == '1':
                    num_islands += 1
                    dfs(r, c)

        return num_islands
```

## Explanation

This problem can be solved using a graph traversal algorithm, either Depth-First Search (DFS) or Breadth-First Search (BFS).

The idea is to iterate through each cell of the grid. If we encounter a '1' (land), it means we've found a part of an island. We increment our island count and then use DFS (or BFS) to explore the entire connected component of land cells belonging to this island. As we visit each land cell, we mark it as '0' (or any other visited marker, like '2' in the solution) to ensure we don't count it again or get stuck in an infinite loop.

**DFS Approach:**

1.  **Initialization:** Initialize `num_islands` to 0.
2.  **Iterate Grid:** Loop through each cell `(r, c)` in the grid.
3.  **Find Land:** If `grid[r][c]` is '1':
    -   Increment `num_islands`.
    -   Call a `dfs` helper function starting from `(r, c)`.
4.  **`dfs` Helper Function:**
    -   **Base Cases:** The `dfs` function stops if it goes out of bounds, encounters a '0' (water), or encounters a cell that has already been visited.
    -   **Mark Visited:** If the current cell is '1', mark it as visited (e.g., change it to '2').
    -   **Explore Neighbors:** Recursively call `dfs` for all four adjacent neighbors (up, down, left, right).

By marking visited land cells, we ensure that each island is counted exactly once.

The time complexity is O(M * N) because we visit each cell in the grid at most once. The space complexity is O(M * N) in the worst case for the recursion stack of DFS (e.g., a grid full of land).
