
# 41. Longest Palindrome

## Problem Statement

Given a string `s` which consists of lowercase or uppercase English letters, return *the length of the longest palindrome that can be built with those letters*.

Letters are case sensitive, for example, `"Aa"` is not considered a palindrome here.

**Example 1:**

**Input:** s = "abccccdd"
**Output:** 7
**Explanation:** One longest palindrome that can be built is "dccaccd", whose length is 7.

**Example 2:**

**Input:** s = "a"
**Output:** 1

**Example 3:**

**Input:** s = "bb"
**Output:** 2

## Solution

```python
from collections import Counter

class Solution:
    def longestPalindrome(self, s: str) -> int:
        char_counts = Counter(s)
        length = 0
        has_odd = False

        for count in char_counts.values():
            if count % 2 == 0:
                length += count
            else:
                # If a character appears an odd number of times,
                # we can use all but one of its occurrences to form pairs.
                length += count - 1
                # We can use one character with an odd count as the center
                # of the palindrome.
                has_odd = True

        # If there was at least one character with an odd count,
        # we can add 1 to the total length for the center character.
        if has_odd:
            length += 1

        return length
```

## Explanation

To form the longest possible palindrome from a given set of characters, we can use all characters that appear an even number of times. For characters that appear an odd number of times, we can use all but one of their occurrences to form pairs, and then use one of these characters as the center of the palindrome.

**Algorithm:**

1.  **Count Character Frequencies:** Use a `Counter` (or a hash map) to count the occurrences of each character in the input string `s`.

2.  **Build Palindrome Length:** Iterate through the counts of each character:
    -   If a character's `count` is even, we can use all of its occurrences to form pairs, so we add `count` to our `length`.
    -   If a character's `count` is odd, we can use `count - 1` of its occurrences to form pairs. We add `count - 1` to our `length`. We also set a flag `has_odd` to `True`, indicating that we have at least one character that can serve as the center of the palindrome.

3.  **Add Center Character:** After iterating through all characters, if `has_odd` is `True`, it means we have at least one character that appeared an odd number of times. We can use one of these as the central character of our palindrome, increasing the total `length` by 1.

**Example:** `s = "abccccdd"`

-   `a`: 1 (odd) -> `length = 0`, `has_odd = True`
-   `b`: 1 (odd) -> `length = 0`, `has_odd = True`
-   `c`: 4 (even) -> `length = 4`
-   `d`: 2 (even) -> `length = 4 + 2 = 6`

After loop: `length = 6`, `has_odd = True`. Add 1 for the center: `length = 7`.

This approach has a time complexity of O(n) for counting characters and O(k) for iterating through the unique characters (where k is the size of the alphabet, which is constant). So, overall O(n). The space complexity is O(k) for the `Counter`.
