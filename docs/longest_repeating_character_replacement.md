
# 34. Longest Repeating Character Replacement

## Problem Statement

You are given a string `s` and an integer `k`.

You can choose any character of the string and change it to any other uppercase English character any number of times.

You can perform this operation at most `k` times.

Return *the length of the longest substring containing the same letter you can get after performing the above operations*.

**Example 1:**

**Input:** s = "ABAB", k = 2
**Output:** 4
**Explanation:** Replace the two 'A's with two 'B's or vice versa.

**Example 2:**

**Input:** s = "AABABBA", k = 1
**Output:** 4
**Explanation:** Replace the one 'A' in the middle with 'B' and form "AABBBBA".
The substring "BBBB" has a length of 4.

## Solution

```python
from collections import defaultdict

class Solution:
    def characterReplacement(self, s: str, k: int) -> int:
        char_counts = defaultdict(int)
        left = 0
        max_freq = 0
        max_len = 0

        for right in range(len(s)):
            char_counts[s[right]] += 1
            max_freq = max(max_freq, char_counts[s[right]])

            # Current window size - count of most frequent char in window
            # This difference is the number of characters that need to be replaced.
            # If this is greater than k, we need to shrink the window.
            if (right - left + 1) - max_freq > k:
                char_counts[s[left]] -= 1
                left += 1

            max_len = max(max_len, right - left + 1)

        return max_len
```

## Explanation

This problem can be solved using a sliding window approach.

The goal is to find the longest substring where, after at most `k` replacements, all characters in that substring become the same.

**Key Idea:** In any valid window (substring), the number of characters that are *not* the most frequent character in that window must be less than or equal to `k`.

1.  **Sliding Window:** We use a window defined by `left` and `right` pointers.
2.  **Character Counts:** We maintain a frequency map (`char_counts`) of characters within the current window.
3.  **`max_freq`:** We also keep track of `max_freq`, which is the count of the most frequent character within the current window.

**Algorithm:**

-   We expand the window by moving `right` one step at a time.
-   For each `s[right]`, we increment its count in `char_counts` and update `max_freq`.
-   We then check the condition: `(current_window_size - max_freq) > k`.
    -   `current_window_size` is `right - left + 1`.
    -   `current_window_size - max_freq` represents the number of characters in the current window that are *different* from the most frequent character. These are the characters we would need to replace.
    -   If this value exceeds `k`, it means our current window is invalid. We need to shrink the window from the `left` side by decrementing the count of `s[left]` and incrementing `left`.
-   We update `max_len` with the maximum length of a valid window found so far.

This greedy approach works because we are always trying to expand the window as much as possible. When the window becomes invalid, we shrink it just enough to make it valid again. The `max_freq` doesn't necessarily need to be re-calculated for the entire window when `left` moves, because if a previous `max_freq` was higher, it means we had a longer valid window at that point, and we are only interested in the maximum length.

The time complexity is O(n) because both `left` and `right` pointers traverse the string at most once. The space complexity is O(1) because the `char_counts` map will store at most 26 uppercase English letters.
