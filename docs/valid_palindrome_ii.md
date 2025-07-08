# 40. Valid Palindrome II

## Problem Statement

Given a string `s`, return `true` if the `s` can be a palindrome after deleting **at most one** character from it.

**Example 1:**

**Input:** s = "aba"
**Output:** true

**Example 2:**

**Input:** s = "abca"
**Output:** true
**Explanation:** You could delete the character 'c'.

**Example 3:**

**Input:** s = "abc"
**Output:** false

## Solution

```python
class Solution:
    def validPalindrome(self, s: str) -> bool:
        left, right = 0, len(s) - 1

        while left < right:
            if s[left] != s[right]:
                # Mismatch found. Try deleting s[left] or s[right]
                # Check if s[left+1...right] is a palindrome
                # Check if s[left...right-1] is a palindrome
                return self.is_palindrome_range(s, left + 1, right) or \
                       self.is_palindrome_range(s, left, right - 1)
            left += 1
            right -= 1

        return True

    def is_palindrome_range(self, s: str, left: int, right: int) -> bool:
        while left < right:
            if s[left] != s[right]:
                return False
            left += 1
            right -= 1
        return True
```

## Explanation

This problem extends the basic palindrome check. We are allowed to delete at most one character.

The approach uses a two-pointer technique.

1.  **Initial Two-Pointer Scan:** We use two pointers, `left` starting at the beginning and `right` starting at the end of the string `s`.

2.  **Mismatch Handling:** We iterate inward as long as `s[left]` equals `s[right]`.
    -   If `s[left]` and `s[right]` are **not** equal, it means we've found a character that needs to be potentially deleted. At this point, we have two choices:
        1.  **Delete `s[left]`:** Check if the substring `s[left+1 ... right]` is a palindrome.
        2.  **Delete `s[right]`:** Check if the substring `s[left ... right-1]` is a palindrome.
    -   If either of these two resulting substrings is a palindrome, then the original string `s` can be made a palindrome by deleting at most one character, so we return `True`.

3.  **`is_palindrome_range` Helper:** A helper function `is_palindrome_range(s, left, right)` is used to check if a given substring (defined by `left` and `right` indices) is a palindrome.

4.  **No Mismatch:** If the `while` loop completes without finding any mismatches, it means the original string `s` is already a palindrome, so we return `True`.

The time complexity is O(n) because in the worst case, we traverse the string once with the main loop, and then potentially twice more with the `is_palindrome_range` helper, each taking O(n) time. The space complexity is O(1).

