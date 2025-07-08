
# 38. Valid Parentheses

## Problem Statement

Given a string `s` containing just the characters `'('`, `')'`, `'{ '`, `'}'`, `'['`, and `']'`, determine if the input string is valid.

An input string is valid if:

1.  Open brackets must be closed by the same type of open brackets.
2.  Open brackets must be closed in the correct order.
3.  Every close bracket has a corresponding open bracket of the same type.

**Example 1:**

**Input:** s = "()"
**Output:** true

**Example 2:**

**Input:** s = "()[]{}"
**Output:** true

**Example 3:**

**Input:** s = "(]"
**Output:** false

## Solution

```python
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []
        # Map opening brackets to their corresponding closing brackets
        mapping = {")": "(", "}": "{", "]": "["}

        for char in s:
            if char in mapping.values():  # It's an opening bracket
                stack.append(char)
            elif char in mapping:  # It's a closing bracket
                # If stack is empty or top of stack doesn't match
                if not stack or stack.pop() != mapping[char]:
                    return False
            else:
                # Character is not a valid bracket
                return False

        # If the stack is empty, all brackets were matched
        return not stack
```

## Explanation

This problem is a classic application of a stack data structure.

The idea is to process the string character by character:

1.  **Initialize a Stack:** Create an empty stack to store opening brackets.
2.  **Define Mapping:** Create a dictionary or hash map that maps each closing bracket to its corresponding opening bracket (e.g., `')' -> '('`).

**Algorithm:**

-   Iterate through each `char` in the input string `s`:
    -   **If `char` is an opening bracket:** Push it onto the stack.
    -   **If `char` is a closing bracket:**
        -   Check if the stack is empty. If it is, it means we encountered a closing bracket without a corresponding opening bracket, so the string is invalid (return `False`).
        -   Pop the top element from the stack. This should be the most recently encountered opening bracket.
        -   Compare the popped opening bracket with the expected opening bracket for the current closing bracket (using our `mapping`). If they don't match, the order is incorrect, so the string is invalid (return `False`).
    -   **If `char` is not a valid bracket:** (Optional, depending on problem constraints, but good for robustness) return `False`.

-   **Final Check:** After iterating through the entire string, if the stack is empty, it means all opening brackets have been correctly matched with their closing counterparts. If the stack is not empty, it means there are unmatched opening brackets, so the string is invalid.

The time complexity is O(n) because we iterate through the string once. The space complexity is O(n) in the worst case (e.g., a string with all opening brackets) for the stack.
