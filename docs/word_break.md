
# 20. Word Break

## Problem Statement

Given a string `s` and a dictionary of strings `wordDict`, return `true` if `s` can be segmented into a space-separated sequence of one or more dictionary words.

**Note** that the same word in the dictionary may be reused multiple times in the segmentation.

**Example 1:**

**Input:** s = "leetcode", wordDict = ["leet","code"]
**Output:** true
**Explanation:** Return true because "leetcode" can be segmented as "leet code".

**Example 2:**

**Input:** s = "applepenapple", wordDict = ["apple","pen"]
**Output:** true
**Explanation:** Return true because "applepenapple" can be segmented as "apple pen apple". Note that you are allowed to reuse a dictionary word.

**Example 3:**

**Input:** s = "catsandog", wordDict = ["cats","dog","sand","and","cat"]
**Output:** false

## Solution

```python
class Solution:
    def wordBreak(self, s: str, wordDict: list[str]) -> bool:
        word_set = set(wordDict)
        n = len(s)

        # dp[i] will be true if s[0..i-1] can be segmented
        dp = [False] * (n + 1)
        dp[0] = True  # Base case: an empty string can always be segmented

        for i in range(1, n + 1):
            for j in range(i):
                # Check if the substring s[j:i] is in the dictionary
                # and if the prefix s[0:j] is also breakable
                if dp[j] and s[j:i] in word_set:
                    dp[i] = True
                    break  # Found a valid break, move to the next i

        return dp[n]
```

## Explanation

This problem can be solved using dynamic programming.

Let `dp[i]` be a boolean value indicating whether the prefix of `s` of length `i` (`s[0...i-1]`) can be segmented into words from the dictionary.

1.  **Initialization:** We create a `dp` array of size `n+1`. `dp[0]` is set to `True` because an empty string can be considered segmented.

2.  **DP Relation:** We iterate from `i = 1` to `n`. For each `i`, we check all possible split points `j` before it (`0 <= j < i`).
    -   The prefix `s[0...i-1]` can be segmented if:
        1.  The prefix `s[0...j-1]` can be segmented (i.e., `dp[j]` is `True`).
        2.  The remaining substring `s[j...i-1]` is a word in our dictionary.

3.  **Optimization:** We convert `wordDict` to a set for O(1) average time complexity lookups.

4.  **Final Result:** `dp[n]` will tell us if the entire string `s` can be segmented.

The time complexity is O(n^2) due to the nested loops. The space complexity is O(n) for the `dp` array.
