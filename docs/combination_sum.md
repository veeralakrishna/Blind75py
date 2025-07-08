
# 21. Combination Sum

## Problem Statement

Given an array of **distinct** integers `candidates` and a target integer `target`, return *a list of all **unique combinations** of `candidates` where the chosen numbers sum to `target`*. You may return the combinations in **any order**.

The **same** number may be chosen from `candidates` an **unlimited number of times**. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

The test cases are generated such that the number of unique combinations that sum up to `target` is less than `150` combinations for the given input.

**Example 1:**

**Input:** candidates = [2,3,6,7], target = 7
**Output:** [[2,2,3],[7]]
**Explanation:**
2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.

**Example 2:**

**Input:** candidates = [2,3,5], target = 8
**Output:** [[2,2,2,2],[2,3,3],[3,5]]

**Example 3:**

**Input:** candidates = [2], target = 1
**Output:** []

## Solution

```python
class Solution:
    def combinationSum(self, candidates: list[int], target: int) -> list[list[int]]:
        result = []

        def backtrack(remaining, combination, start):
            if remaining == 0:
                # Found a valid combination
                result.append(list(combination))
                return

            if remaining < 0:
                # Exceeded the target
                return

            for i in range(start, len(candidates)):
                # Add the number to the current combination
                combination.append(candidates[i])
                # Continue exploring with the same number (since we can reuse it)
                backtrack(remaining - candidates[i], combination, i)
                # Backtrack: remove the number to explore other possibilities
                combination.pop()

        backtrack(target, [], 0)
        return result
```

## Explanation

This problem is a classic backtracking problem.

The `backtrack` function explores all possible combinations recursively.

1.  **Base Cases:**
    -   If `remaining` (the target sum we still need to reach) is 0, it means we have found a valid combination. We add a copy of the current `combination` to our `result`.
    -   If `remaining` is less than 0, it means the current path is invalid, so we stop exploring it.

2.  **Recursive Step:**
    -   We iterate through the `candidates` starting from the `start` index. The `start` index is used to ensure that we don't generate duplicate combinations (e.g., `[2,3]` and `[3,2]` if we always started from index 0).
    -   For each candidate, we add it to our current `combination`.
    -   We then make a recursive call to `backtrack`, reducing the `remaining` sum and passing the same `start` index (`i`) because we are allowed to reuse the same element.
    -   After the recursive call returns, we **backtrack** by removing the last added element (`combination.pop()`). This allows us to explore other branches of the decision tree.

The time complexity is hard to define precisely but is exponential, roughly O(N^(T/M + 1)), where N is the number of candidates, T is the target, and M is the minimum candidate value. The space complexity is O(T/M) for the recursion depth.
