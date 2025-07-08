
# 32. Palindromic Substrings

## Problem Statement

Given a string `s`, return *the number of palindromic substrings in it*.

A string is a **palindrome** when it reads the same backward as forward.

A **substring** is a contiguous sequence of characters within the string.

**Example 1:**

**Input:** s = "abc"
**Output:** 3
**Explanation:** Three palindromic strings: "a", "b", "c".

**Example 2:**

**Input:** s = "aaa"
**Output:** 6
**Explanation:** Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".

## Solution

```python
class Solution:
    def countSubstrings(self, s: str) -> int:
        count = 0
        n = len(s)

        def expand_around_center(left, right):
            nonlocal count
            while left >= 0 and right < n and s[left] == s[right]:
                count += 1  # Found a new palindromic substring
                left -= 1
                right += 1

        for i in range(n):
            # Odd length palindromes (e.g., "aba"), center is s[i]
            expand_around_center(i, i)
            # Even length palindromes (e.g., "abba"), center is between s[i] and s[i+1]
            expand_around_center(i, i + 1)

        return count
```

## Explanation

This problem is a variation of the "Longest Palindromic Substring" problem. Instead of finding the longest one, we need to count all of them.

The "expand around center" approach is very suitable here.

1.  **Iterate through potential centers:** Every single character in the string can be the center of an odd-length palindrome (e.g., `a` in `b_a_b`). Every pair of adjacent characters can be the center of an even-length palindrome (e.g., `bb` in `a_bb_a`).

2.  **`expand_around_center` function:**
    -   This helper function takes `left` and `right` pointers, representing the initial center(s).
    -   It expands outwards from these pointers as long as the characters match and are within the string boundaries.
    -   Crucially, **every time a match is found and the pointers are expanded, it means we have found a new palindromic substring**. So, we increment our `count`.

3.  **Total Count:** By calling `expand_around_center` for every possible single-character center (`i, i`) and every possible two-character center (`i, i+1`), we ensure that all palindromic substrings are found and counted.

The time complexity is O(n^2) because we iterate through each character (n) and for each character, we expand outwards (up to n/2 times). The space complexity is O(1).
