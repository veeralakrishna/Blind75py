
# 42. Permutation in String

## Problem Statement

Given two strings `s1` and `s2`, return `true` if `s2` contains one of the permutations of `s1`, or `false` otherwise.

In other words, return `true` if one of `s1`'s permutations is the substring of `s2`.

**Example 1:**

**Input:** s1 = "ab", s2 = "eidbaooo"
**Output:** true
**Explanation:** s2 contains one permutation of s1 ("ba").

**Example 2:**

**Input:** s1 = "ab", s2 = "eidboaoo"
**Output:** false

## Solution

```python
from collections import Counter

class Solution:
    def checkInclusion(self, s1: str, s2: str) -> bool:
        len1, len2 = len(s1), len(s2)

        if len1 > len2:
            return False

        # Count character frequencies for s1
        s1_counts = Counter(s1)
        # Count character frequencies for the initial window in s2
        window_counts = Counter(s2[:len1])

        # Check if the initial window is a permutation
        if s1_counts == window_counts:
            return True

        # Slide the window across s2
        for i in range(len1, len2):
            # Add the new character entering the window
            window_counts[s2[i]] += 1
            # Remove the character leaving the window
            window_counts[s2[i - len1]] -= 1

            # If a character count becomes zero, remove it from the counter
            # to ensure accurate comparison with s1_counts
            if window_counts[s2[i - len1]] == 0:
                del window_counts[s2[i - len1]]

            # Check if the current window is a permutation
            if s1_counts == window_counts:
                return True

        return False
```

## Explanation

This problem can be solved using a sliding window approach combined with frequency maps.

**Core Idea:** A string `s2` contains a permutation of `s1` if there is a substring in `s2` that has the exact same character counts as `s1`.

1.  **Initial Setup:**
    -   Check if `len(s1)` is greater than `len(s2)`. If so, a permutation of `s1` cannot exist in `s2`, so return `False`.
    -   Create a frequency map (`s1_counts`) for `s1`.
    -   Create a frequency map (`window_counts`) for the initial window in `s2` (of length `len(s1)`).
    -   Compare `s1_counts` and `window_counts`. If they are equal, we found a permutation.

2.  **Sliding Window:**
    -   Iterate from `len1` to `len2 - 1` (the end of `s2`). In each iteration, we simulate sliding the window one position to the right.
    -   **Add Character:** Add the new character `s2[i]` (entering the window) to `window_counts`.
    -   **Remove Character:** Remove the character `s2[i - len1]` (leaving the window) from `window_counts`. If its count becomes 0, delete it from the map to ensure accurate comparison.
    -   **Compare:** After updating `window_counts`, compare it with `s1_counts`. If they are equal, return `True`.

3.  **Return False:** If the loop completes without finding a matching window, return `False`.

**Time and Space Complexity:**

-   **Time Complexity:** O(L1 + L2), where L1 is `len(s1)` and L2 is `len(s2)`. We iterate through `s1` once to build its frequency map, and then iterate through `s2` once with the sliding window. Dictionary operations (add, remove, compare) take O(alphabet_size) time, which is constant (26 for lowercase English letters).
-   **Space Complexity:** O(alphabet_size) for storing the frequency maps.
