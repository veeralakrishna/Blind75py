
# 35. Minimum Window Substring

## Problem Statement

Given two strings `s` and `t` of lengths `m` and `n` respectively, return *the **minimum window substring** of `s` such that every character in `t` (including duplicates) is included in the window*. If there is no such substring, return the empty string `""`.

The test cases will be generated such that the answer is unique.

A **substring** is a contiguous sequence of characters within a string.

**Example 1:**

**Input:** s = "ADOBECODEBANC", t = "ABC"
**Output:** "BANC"
**Explanation:** The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.

**Example 2:**

**Input:** s = "a", t = "a"
**Output:** "a"
**Explanation:** The entire string s is the minimum window.

**Example 3:**

**Input:** s = "a", t = "aa"
**Output:** ""
**Explanation:** Both 'a's from t must be included in the window. Since s only has one 'a', no such window exists.

## Solution

```python
from collections import Counter

class Solution:
    def minWindow(self, s: str, t: str) -> str:
        if not t or not s:
            return ""

        # Dictionary to store frequency of characters in t
        dict_t = Counter(t)

        # Number of unique characters in t that must be present in window
        required = len(dict_t)

        # left and right pointers for the sliding window
        left = 0

        # formed is the number of unique characters in the current window
        # that match the required count in dict_t.
        formed = 0

        # Dictionary to store frequency of characters in the current window
        window_counts = {}

        # ans tuple of the form (window length, left, right)
        ans = float('inf'), None, None

        for right in range(len(s)):
            character = s[right]
            window_counts[character] = window_counts.get(character, 0) + 1

            # If the current character's count in window matches its count in t
            if character in dict_t and window_counts[character] == dict_t[character]:
                formed += 1

            # Try to contract the window from the left
            while left <= right and formed == required:
                character = s[left]

                # Update the minimum window found so far
                if right - left + 1 < ans[0]:
                    ans = (right - left + 1, left, right)

                # Remove character from the left of the window
                window_counts[character] -= 1
                if character in dict_t and window_counts[character] < dict_t[character]:
                    formed -= 1

                left += 1

        return "" if ans[0] == float('inf') else s[ans[1] : ans[2] + 1]
```

## Explanation

This problem is a classic application of the sliding window technique.

**Core Idea:** We expand the window from the right until it contains all characters of `t`. Once it does, we try to shrink it from the left to find the smallest valid window. We keep track of the best (smallest) window found so far.

1.  **Character Frequencies:**
    -   `dict_t`: Stores the frequency of each character in string `t`.
    -   `window_counts`: Stores the frequency of each character in the current sliding window.

2.  **`required` and `formed`:**
    -   `required`: The number of unique characters from `t` that we need to have in our window (with at least their required frequencies).
    -   `formed`: The number of unique characters in the current window that satisfy their frequency requirement from `dict_t`.

**Algorithm:**

-   **Expand Window (Right Pointer):** The `right` pointer iterates through `s`, adding characters to `window_counts`. If a character `s[right]` is in `dict_t` and its count in `window_counts` matches `dict_t`, we increment `formed`.

-   **Contract Window (Left Pointer):** Once `formed == required` (meaning the current window contains all characters of `t` with their required frequencies), we enter a `while` loop to try and shrink the window from the `left`.
    -   Inside this loop, we first check if the current window is smaller than the `ans` found so far and update `ans` if it is.
    -   Then, we remove `s[left]` from `window_counts` and increment `left`. If `s[left]` was a character from `t` and its count in `window_counts` drops below its required count in `dict_t`, we decrement `formed`.

-   The `while` loop continues as long as `formed == required`, meaning we keep shrinking the window from the left until it becomes invalid (i.e., `formed < required`). At that point, we resume expanding the window from the right.

This approach ensures that we find the minimum window. The time complexity is O(S + T) where S is the length of `s` and T is the length of `t`, because both pointers traverse `s` at most once, and `Counter(t)` takes O(T). The space complexity is O(alphabet_size) for the dictionaries.
