
# 18. Longest Increasing Subsequence

## Problem Statement

Given an integer array `nums`, return *the length of the longest strictly increasing subsequence*.

**Example 1:**

**Input:** nums = [10,9,2,5,3,7,101,18]
**Output:** 4
**Explanation:** The longest increasing subsequence is [2,3,7,101], therefore the length is 4.

**Example 2:**

**Input:** nums = [0,1,0,3,2,3]
**Output:** 4

**Example 3:**

**Input:** nums = [7,7,7,7,7,7,7]
**Output:** 1

## Solution

```python
import bisect

class Solution:
    def lengthOfLIS(self, nums: list[int]) -> int:
        # This list will store the smallest tail of all increasing subsequences
        # with length i+1 in tails[i].
        tails = []
        for num in nums:
            # Find the first position where num can be inserted to maintain order
            i = bisect.bisect_left(tails, num)
            if i == len(tails):
                # If num is greater than all elements in tails, extend the subsequence
                tails.append(num)
            else:
                # Replace the tail of an existing subsequence with a smaller number
                # This allows for potentially longer subsequences later on
                tails[i] = num
        return len(tails)

```

## Explanation

This problem can be solved with a clever approach that uses both dynamic programming and binary search, resulting in an O(n log n) time complexity.

The core idea is to maintain a list called `tails`. This list stores the smallest tail of all increasing subsequences with a certain length. `tails[i]` will be the smallest tail of an increasing subsequence of length `i+1`.

Let's trace `nums = [10, 9, 2, 5, 3, 7, 101, 18]`:

1.  `num = 10`: `tails` is `[]`. `i=0`. `tails` becomes `[10]`.
2.  `num = 9`: `tails` is `[10]`. `i=0`. `tails` becomes `[9]` (replace 10).
3.  `num = 2`: `tails` is `[9]`. `i=0`. `tails` becomes `[2]` (replace 9).
4.  `num = 5`: `tails` is `[2]`. `i=1`. `tails` becomes `[2, 5]`.
5.  `num = 3`: `tails` is `[2, 5]`. `i=1`. `tails` becomes `[2, 3]` (replace 5).
6.  `num = 7`: `tails` is `[2, 3]`. `i=2`. `tails` becomes `[2, 3, 7]`.
7.  `num = 101`: `tails` is `[2, 3, 7]`. `i=3`. `tails` becomes `[2, 3, 7, 101]`.
8.  `num = 18`: `tails` is `[2, 3, 7, 101]`. `i=3`. `tails` becomes `[2, 3, 7, 18]` (replace 101).

The final `tails` list is `[2, 3, 7, 18]`. The length of this list, 4, is the length of the longest increasing subsequence.

We use `bisect.bisect_left` to perform the binary search efficiently. This finds the insertion point for `num` in `tails` to keep it sorted. This is the key to the O(n log n) performance.

The space complexity is O(n) for the `tails` list.
