
# 36. Valid Anagram

## Problem Statement

Given two strings `s` and `t`, return `true` if `t` is an anagram of `s`, and `false` otherwise.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

**Example 1:**

**Input:** s = "anagram", t = "nagaram"
**Output:** true

**Example 2:**

**Input:** s = "rat", t = "car"
**Output:** false

## Solution

```python
from collections import Counter

class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False

        # Use Counter to get frequency maps of characters
        count_s = Counter(s)
        count_t = Counter(t)

        # Compare the frequency maps
        return count_s == count_t
```

## Explanation

An anagram is formed by rearranging the letters of another word or phrase. This implies two conditions:

1.  Both strings must have the same length.
2.  Both strings must contain the same characters with the same frequencies.

**Approach using Hash Maps (Counters):**

1.  **Length Check:** First, we check if the lengths of `s` and `t` are different. If they are, they cannot be anagrams, so we return `False`.

2.  **Frequency Counting:** We use `collections.Counter` (which is a specialized dictionary subclass) to count the frequency of each character in both strings `s` and `t`.

3.  **Comparison:** Finally, we compare the two `Counter` objects. If they are equal, it means both strings have the same characters with the same frequencies, and thus `t` is an anagram of `s`. Otherwise, it's not.

This approach is efficient because `Counter` builds the frequency maps in O(n) time (where n is the length of the string), and comparing two `Counter` objects also takes O(alphabet_size) time. So, the overall time complexity is O(n). The space complexity is O(alphabet_size) for storing the character counts.

**Alternative (Sorting):**

Another simple approach is to sort both strings and then compare them. If they are anagrams, their sorted versions will be identical.

```python
class Solution:
    def isAnagram_sort(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False
        return sorted(s) == sorted(t)
```

This sorting approach has a time complexity of O(n log n) due to the sorting, and space complexity depends on the sorting algorithm (typically O(n) or O(log n)). The `Counter` approach is generally preferred for better performance.
