
# 39. Valid Palindrome

## Problem Statement

A phrase is a **palindrome** if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.

Given a string `s`, return `true` *if it is a palindrome, or* `false` *otherwise*.

**Example 1:**

**Input:** s = "A man, a plan, a canal: Panama"
**Output:** true
**Explanation:** "amanaplanacanalpanama" is a palindrome.

**Example 2:**

**Input:** s = "race a car"
**Output:** false
**Explanation:** "raceacar" is not a palindrome.

**Example 3:**

**Input:** s = " "
**Output:** true
**Explanation:** s is an empty string "". After removing non-alphanumeric characters and converting to lowercase, it becomes an empty string. Since an empty string reads the same forward and backward, it is a palindrome.

## Solution

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        # Create a new string with only alphanumeric characters, converted to lowercase
        filtered_chars = [
            char.lower() for char in s if char.isalnum()
        ]
        filtered_s = "".join(filtered_chars)

        # Check if the filtered string is a palindrome
        return filtered_s == filtered_s[::-1]
```

## Explanation

This problem requires us to first preprocess the input string and then check if the processed string is a palindrome.

**Preprocessing Steps:**

1.  **Filter Alphanumeric Characters:** Iterate through the input string `s` and keep only characters that are alphanumeric (letters or numbers). We can use the `isalnum()` string method for this.
2.  **Convert to Lowercase:** Convert all the filtered characters to lowercase. This ensures that case doesn't affect the palindrome check (e.g., 'A' and 'a' are treated the same).
3.  **Join Characters:** Join the filtered and lowercased characters back into a new string.

**Palindrome Check:**

Once we have the `filtered_s` string, we can check if it's a palindrome by comparing it to its reverse. In Python, `[::-1]` is a convenient way to reverse a string.

**Time and Space Complexity:**

-   **Time Complexity:** O(n), where n is the length of the input string `s`. We iterate through the string once to filter and convert characters, and then reversing and comparing the string also takes linear time.
-   **Space Complexity:** O(n) for storing the `filtered_s` string. In the worst case (e.g., all characters are alphanumeric), the new string will have the same length as the original.

**Alternative (Two Pointers - In-place):**

An alternative approach is to use two pointers, one at the beginning and one at the end of the original string. We move the pointers inward, skipping non-alphanumeric characters and comparing the lowercase versions of the alphanumeric characters. This approach can achieve O(1) space complexity (excluding the output string if you were to build one).
