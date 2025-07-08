
# 25. Unique Paths

## Problem Statement

There is a robot on an `m x n` grid. The robot is initially located at the **top-left corner** (i.e., `grid[0][0]`). The robot tries to move to the **bottom-right corner** (i.e., `grid[m - 1][n - 1]`). The robot can only move either down or right at any point in time.

Given the two integers `m` and `n`, return *the number of possible unique paths that the robot can take to reach the bottom-right corner*.

The test cases are generated so that the answer will be less than or equal to `2 * 10^9`.

**Example 1:**

![Unique Paths](https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png)

**Input:** m = 3, n = 7
**Output:** 28

**Example 2:**

**Input:** m = 3, n = 2
**Output:** 3
**Explanation:**
From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Down -> Down
2. Down -> Down -> Right
3. Down -> Right -> Down

## Solution

```python
import math

class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        # This is a combinatorial problem.
        # To reach the bottom-right corner, the robot must make a total of
        # (m - 1) down moves and (n - 1) right moves.
        # The total number of moves is (m - 1) + (n - 1) = m + n - 2.
        # The problem is to choose which of these total moves are "down" moves
        # (the rest will be "right" moves).
        # This is equivalent to "(m + n - 2) choose (m - 1)".

        total_moves = m + n - 2
        down_moves = m - 1

        # Using the formula for combinations: C(n, k) = n! / (k! * (n-k)!)
        return math.comb(total_moves, down_moves)

```

## Explanation

This problem can be solved using dynamic programming, but a more efficient solution comes from combinatorics.

To get from the top-left corner `(0, 0)` to the bottom-right corner `(m-1, n-1)`, the robot must make a total of `(m-1)` moves down and `(n-1)` moves right.

The total number of moves is `(m-1) + (n-1) = m + n - 2`.

The problem then becomes: out of these `m + n - 2` total moves, how many ways can we choose `m-1` of them to be "down" moves? (The remaining `n-1` moves will automatically be "right" moves).

This is a classic combination problem, often denoted as "N choose K" or `C(N, K)`.

Here, `N = m + n - 2` and `K = m - 1` (or `n - 1`, the result is the same).

The formula for combinations is `C(N, K) = N! / (K! * (N-K)!)`.

Python's `math.comb(n, k)` function calculates this directly, providing a very concise and efficient solution.

**Dynamic Programming Approach (for comparison):**

You could also solve this with a 2D DP grid where `dp[i][j]` is the number of paths to cell `(i, j)`. The recurrence would be `dp[i][j] = dp[i-1][j] + dp[i][j-1]`, as you can only arrive from the cell above or the cell to the left. This would have O(m*n) time and space complexity.
