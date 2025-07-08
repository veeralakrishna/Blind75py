
# 33. Longest Substring Without Repeating Characters

## Problem Statement

Given a string `s`, find the length of the **longest substring** without repeating characters.

**Example 1:**

**Input:** s = "abcabcbb"
**Output:** 3
**Explanation:** The answer is "abc", with the length of 3.

**Example 2:**

**Input:** s = "bbbbb"
**Output:** 1
**Explanation:** The answer is "b", with the length of 1.

**Example 3:**

**Input:** s = "pwwkew"
**Output:** 3
**Explanation:** The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.

## Solution

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        char_set = set()
        left = 0
        max_len = 0

        for right in range(len(s)):
            while s[right] in char_set:
                char_set.remove(s[left])
                left += 1
            char_set.add(s[right])
            max_len = max(max_len, right - left + 1)

        return max_len
```

## Explanation

This problem can be efficiently solved using a sliding window approach with a hash set.

1.  **Sliding Window:** We maintain a window `[left, right]` that represents the current substring without repeating characters.
2.  **Hash Set (`char_set`):** We use a hash set to store the characters currently within our window. This allows for O(1) average time complexity for checking if a character is already in the window and for adding/removing characters.

**Algorithm:**

-   We initialize `left` to 0 and `max_len` to 0.
-   We iterate with `right` from the beginning of the string to the end.
-   For each `s[right]`:
    -   **Check for Duplicates:** If `s[right]` is already in `char_set`, it means we have a repeating character. To resolve this, we shrink the window from the `left` side by removing `s[left]` from `char_set` and incrementing `left`, until the duplicate is removed.
    -   **Add Current Character:** Once `s[right]` is not in `char_set` (or after removing the duplicate), we add `s[right]` to `char_set`.
    -   **Update Max Length:** We then update `max_len` with the maximum of its current value and the current window size (`right - left + 1`).

This approach ensures that at any point, our window `[left, right]` contains only unique characters. The time complexity is O(n) because both `left` and `right` pointers traverse the string at most once. The space complexity is O(min(n, alphabet_size)) for the hash set.
