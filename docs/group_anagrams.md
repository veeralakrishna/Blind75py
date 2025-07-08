
# 37. Group Anagrams

## Problem Statement

Given an array of strings `strs`, group the anagrams together. You can return the answer in **any order**.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

**Example 1:**

**Input:** strs = ["eat","tea","tan","ate","nat","bat"]
**Output:** [["bat"],["nat","tan"],["ate","eat","tea"]]

**Example 2:**

**Input:** strs = [""]
**Output:** [[""]]

**Example 3:**

**Input:** strs = ["a"]
**Output:** [["a"]]

## Solution

```python
from collections import defaultdict

class Solution:
    def groupAnagrams(self, strs: list[str]) -> list[list[str]]:
        # Use a defaultdict where keys are sorted tuples of characters
        # and values are lists of anagrams.
        anagram_groups = defaultdict(list)

        for s in strs:
            # Sort the string to create a canonical key for anagrams
            # e.g., "eat" -> "aet", "tea" -> "aet", "ate" -> "aet"
            sorted_s = ''.join(sorted(s))
            anagram_groups[sorted_s].append(s)

        # Return the values (lists of anagrams) from the dictionary
        return list(anagram_groups.values())
```

## Explanation

The core idea to group anagrams is to find a canonical representation for each group of anagrams. Since anagrams contain the same characters with the same frequencies, sorting the characters of an anagram will always result in the same string.

1.  **Canonical Key:** For each string in the input list `strs`, we sort its characters alphabetically. For example, "eat", "tea", and "ate" all become "aet" after sorting. This sorted string acts as a unique key for all its anagrams.

2.  **Hash Map (DefaultDict):** We use a hash map (specifically, `collections.defaultdict(list)` in Python) where:
    -   The **keys** are the sorted strings (our canonical representation).
    -   The **values** are lists of the original strings that produce that sorted key.

3.  **Grouping:** We iterate through the input `strs`. For each string `s`, we sort it to get its `sorted_s` key. We then append the original string `s` to the list associated with `sorted_s` in our `anagram_groups` dictionary.

4.  **Result:** Finally, we extract all the values from the `anagram_groups` dictionary. Each value is a list of strings that are anagrams of each other.

The time complexity is O(N * K log K), where N is the number of strings in `strs` and K is the maximum length of a string. This is because for each of the N strings, we sort it, which takes K log K time. The space complexity is O(N * K) in the worst case, where all strings are unique and stored in the dictionary.
