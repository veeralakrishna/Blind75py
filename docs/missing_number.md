
# 14. Missing Number

## Problem Statement

Given an array `nums` containing `n` distinct numbers in the range `[0, n]`, return *the only number in the range that is missing from the array*.

**Example 1:**

**Input:** nums = [3,0,1]
**Output:** 2
**Explanation:** n = 3 since there are 3 numbers, so all numbers are in the range [0,3]. 2 is the missing number in the range since it does not appear in nums.

**Example 2:**

**Input:** nums = [0,1]
**Output:** 2
**Explanation:** n = 2 since there are 2 numbers, so all numbers are in the range [0,2]. 2 is the missing number in the range since it does not appear in nums.

**Example 3:**

**Input:** nums = [9,6,4,2,3,5,7,0,1]
**Output:** 8
**Explanation:** n = 9 since there are 9 numbers, so all numbers are in the range [0,9]. 8 is the missing number in the range since it does not appear in nums.

## Solution

```python
class Solution:
    def missingNumber(self, nums: list[int]) -> int:
        n = len(nums)
        # Calculate the expected sum of numbers from 0 to n
        expected_sum = n * (n + 1) // 2
        # Calculate the actual sum of numbers in the array
        actual_sum = sum(nums)
        # The difference is the missing number
        return expected_sum - actual_sum
```

## Explanation

This solution uses a mathematical approach based on Gauss's formula for the sum of an arithmetic series.

1.  **Expected Sum:** The input array `nums` has `n` numbers and is missing one number from the range `[0, n]`. If the array were complete, it would contain `n + 1` numbers from 0 to `n`. The sum of such a series is given by the formula `n * (n + 1) / 2`.

2.  **Actual Sum:** We calculate the sum of the numbers actually present in the `nums` array.

3.  **The Difference:** The difference between the `expected_sum` (the sum of a complete series) and the `actual_sum` (the sum of the given numbers) is precisely the missing number.

This approach is very efficient, with a time complexity of O(n) due to the `sum()` function and a space complexity of O(1).

**Alternative (Bit Manipulation):**

Another clever solution uses the XOR operation. The XOR of a number with itself is 0. If we XOR all the numbers from 0 to `n` and all the numbers in the `nums` array, all the numbers that are present in both will cancel out, leaving only the missing number.

```python
class Solution:
    def missingNumber_xor(self, nums: list[int]) -> int:
        missing = len(nums)
        for i, num in enumerate(nums):
            missing ^= i ^ num
        return missing
```
