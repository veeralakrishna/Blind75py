
# 19. Longest Common Subsequence

## Problem Statement

Given two strings `text1` and `text2`, return *the length of their longest **common subsequence**.* If there is no common subsequence, return `0`.

A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

- For example, `"ace"` is a subsequence of `"abcde"`.

A **common subsequence** of two strings is a subsequence that is common to both strings.

**Example 1:**

**Input:** text1 = "abcde", text2 = "ace"
**Output:** 3
**Explanation:** The longest common subsequence is "ace" and its length is 3.

**Example 2:**

**Input:** text1 = "abc", text2 = "abc"
**Output:** 3
**Explanation:** The longest common subsequence is "abc" and its length is 3.

**Example 3:**

**Input:** text1 = "abc", text2 = "def"
**Output:** 0
**Explanation:** There is no common subsequence, so the result is 0.

## Solution

```python
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        m, n = len(text1), len(text2)

        # Create a DP table
        # dp[i][j] will be the length of the LCS of text1[0..i-1] and text2[0..j-1]
        dp = [[0] * (n + 1) for _ in range(m + 1)]

        for i in range(1, m + 1):
            for j in range(1, n + 1):
                if text1[i - 1] == text2[j - 1]:
                    # If characters match, extend the LCS from the previous diagonal
                    dp[i][j] = 1 + dp[i - 1][j - 1]
                else:
                    # If they don't match, take the max from the top or left cell
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])

        return dp[m][n]
```

## Explanation

This is a classic dynamic programming problem that can be solved using a 2D DP table.

Let `dp[i][j]` be the length of the longest common subsequence (LCS) between the first `i` characters of `text1` and the first `j` characters of `text2`.

1.  **Initialization:** We create a `(m+1) x (n+1)` DP table, where `m` and `n` are the lengths of the strings. The table is initialized to zeros.

2.  **DP Relation:** We fill the table based on the following logic:
    -   If the characters `text1[i-1]` and `text2[j-1]` are the same, then the LCS is one character longer than the LCS of the strings without these characters (`text1[0..i-2]` and `text2[0..j-2]`). So, `dp[i][j] = 1 + dp[i-1][j-1]`.
    -   If the characters are different, the LCS is the same as the LCS of either `text1[0..i-1]` and `text2[0..j-2]` ( `dp[i][j-1]` ) or `text1[0..i-2]` and `text2[0..j-1]` ( `dp[i-1][j]` ). We take the maximum of these two.

3.  **Final Result:** The value at `dp[m][n]` will be the length of the LCS for the entire `text1` and `text2`.

The time complexity is O(m*n) due to the nested loops for filling the DP table. The space complexity is also O(m*n) for the table itself.

**(Space Optimization)**

We can optimize the space to O(min(m, n)) because each cell `dp[i][j]` only depends on the previous row and the current row. We can use two rows (or even one row with a temporary variable) to store the necessary values.
