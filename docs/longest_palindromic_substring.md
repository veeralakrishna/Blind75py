
# 31. Longest Palindromic Substring

## Problem Statement

Given a string `s`, return *the longest palindromic substring* in `s`.

**Example 1:**

**Input:** s = "babad"
**Output:** "bab"
**Explanation:** "aba" is also a valid answer.

**Example 2:**

**Input:** s = "cbbd"
**Output:** "bb"

## Solution

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        if not s:
            return ""

        start, end = 0, 0

        def expand_around_center(left, right):
            while left >= 0 and right < len(s) and s[left] == s[right]:
                left -= 1
                right += 1
            return left + 1, right - 1

        for i in range(len(s)):
            # Odd length palindromes (e.g., "aba")
            l1, r1 = expand_around_center(i, i)
            # Even length palindromes (e.g., "abba")
            l2, r2 = expand_around_center(i, i + 1)

            # Compare lengths and update start, end
            if (r1 - l1) > (end - start):
                start, end = l1, r1
            if (r2 - l2) > (end - start):
                start, end = l2, r2

        return s[start : end + 1]
```

## Explanation

This problem can be solved using the "expand around center" approach.

A palindrome is a string that reads the same forwards and backward. This means it has a center. A palindrome can have either an odd length (e.g., "aba", center is 'b') or an even length (e.g., "abba", center is between the two 'b's).

The algorithm iterates through each character of the string, treating it as a potential center of a palindrome. For each character, it considers two cases:

1.  **Odd Length Palindromes:** The center is a single character (`s[i]`). We expand outwards from `i` (`left = i`, `right = i`).
2.  **Even Length Palindromes:** The center is between two characters (`s[i]` and `s[i+1]`). We expand outwards from `i` and `i+1` (`left = i`, `right = i + 1`).

The `expand_around_center` helper function takes `left` and `right` pointers and expands them as long as the characters at `s[left]` and `s[right]` match and are within the string boundaries. It returns the final `left` and `right` indices of the palindrome found.

We keep track of the `start` and `end` indices of the longest palindrome found so far and update them whenever a longer palindrome is discovered.

The time complexity is O(n^2) because we iterate through each character (n) and for each character, we expand outwards (up to n/2 times). The space complexity is O(1).
