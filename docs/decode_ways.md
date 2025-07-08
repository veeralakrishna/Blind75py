
# 24. Decode Ways

## Problem Statement

A message containing letters from `A-Z` can be **encoded** into numbers using the following mapping:

'A' -> "1"
'B' -> "2"
...
'Z' -> "26"

To **decode** an encoded message, all the digits must be grouped then mapped back into letters using the reverse of the mapping above (there may be multiple ways). For example, `"11106"` can be mapped into:

- `"AAJF"` with the grouping `(1 1 10 6)`
- `"KJF"` with the grouping `(11 10 6)`

Note that the grouping `(1 11 06)` is invalid because `"06"` cannot be mapped into `'F'` since `"6"` is different from `"06"`.

Given a string `s` containing only digits, return *the **number** of ways to decode it*.

The test cases are generated so that the answer fits in a **32-bit** integer.

**Example 1:**

**Input:** s = "12"
**Output:** 2
**Explanation:** "12" could be decoded as "AB" (1 2) or "L" (12).

**Example 2:**

**Input:** s = "226"
**Output:** 3
**Explanation:** "226" could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).

**Example 3:**

**Input:** s = "06"
**Output:** 0
**Explanation:** "06" cannot be mapped to "F" because of the leading zero ("6" is different from "06").

## Solution

```python
class Solution:
    def numDecodings(self, s: str) -> int:
        if not s or s[0] == '0':
            return 0

        # dp[i] = number of ways to decode s[:i]
        dp = [0] * (len(s) + 1)
        dp[0] = 1  # Base case: empty string has one decoding
        dp[1] = 1  # s[0] is not '0', so one way to decode

        for i in range(2, len(s) + 1):
            # One-digit decode
            if s[i-1] != '0':
                dp[i] += dp[i-1]

            # Two-digit decode
            two_digit = int(s[i-2:i])
            if 10 <= two_digit <= 26:
                dp[i] += dp[i-2]

        return dp[-1]
```

## Explanation

This problem can be solved using dynamic programming.

Let `dp[i]` be the number of ways to decode the prefix of the string `s` of length `i` (`s[:i]`).

1.  **Initialization:**
    -   `dp[0] = 1`: There is one way to decode an empty string (the empty decoding).
    -   `dp[1] = 1`: If the first character `s[0]` is not '0', there is one way to decode it.

2.  **DP Relation:** We iterate from `i = 2` to `len(s)`. For each `i`, we consider the last one or two digits:
    -   **One-Digit Decode:** We look at the last digit `s[i-1]`. If it's not '0', it can be decoded as a single letter. The number of ways to do this is the same as the number of ways to decode the string up to the previous character, so we add `dp[i-1]` to `dp[i]`.
    -   **Two-Digit Decode:** We look at the last two digits `s[i-2:i]`. If this two-digit number is between 10 and 26 (inclusive), it can be decoded as a single letter. The number of ways to do this is the same as the number of ways to decode the string up to two characters before, so we add `dp[i-2]` to `dp[i]`.

3.  **Final Result:** `dp[len(s)]` will contain the total number of ways to decode the entire string.

The time complexity is O(n) because we iterate through the string once. The space complexity is O(n) for the `dp` array. This can be optimized to O(1) space by only storing the last two `dp` values.
