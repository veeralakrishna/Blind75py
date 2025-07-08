
# 75. Non-overlapping Intervals

## Problem Statement

Given an array of `intervals` where `intervals[i] = [starti, endi]`, return *the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping*.

**Example 1:**

**Input:** intervals = [[1,2],[2,3],[3,4],[1,3]]
**Output:** 1
**Explanation:** [1,3] can be removed and the rest of the intervals are non-overlapping.

**Example 2:**

**Input:** intervals = [[1,2],[1,2],[1,2]]
**Output:** 2
**Explanation:** You need to remove two [1,2] intervals to make the rest non-overlapping.

**Example 3:**

**Input:** intervals = [[1,2],[2,3]]
**Output:** 0
**Explanation:** You don't need to remove any of the intervals since they're already non-overlapping.

## Solution

```python
class Solution:
    def eraseOverlapIntervals(self, intervals: list[list[int]]) -> int:
        if not intervals:
            return 0

        # Sort intervals by their end times
        # This greedy approach works because choosing the interval that ends earliest
        # leaves the maximum room for subsequent non-overlapping intervals.
        intervals.sort(key=lambda x: x[1])

        end = intervals[0][1]
        count = 1 # Start with one non-overlapping interval

        for i in range(1, len(intervals)):
            # If the current interval's start time is greater than or equal to
            # the end time of the last chosen non-overlapping interval,
            # then it does not overlap and can be included.
            if intervals[i][0] >= end:
                end = intervals[i][1]
                count += 1

        # The minimum number of intervals to remove is total intervals - max non-overlapping intervals
        return len(intervals) - count
```

## Explanation

This problem asks for the minimum number of intervals to remove to make the remaining intervals non-overlapping. This is equivalent to finding the maximum number of non-overlapping intervals and then subtracting that from the total number of intervals.

**Core Idea (Greedy Approach):** To maximize the number of non-overlapping intervals, we should always pick the interval that ends earliest among the available non-overlapping choices. This leaves the maximum possible space for subsequent intervals.

1.  **Sort by End Times:** The crucial step is to sort the `intervals` based on their **end times**. If two intervals have the same end time, their start times can be used as a tie-breaker (though not strictly necessary for correctness in this specific problem).

2.  **Iterate and Select Non-Overlapping:**
    -   Initialize `end` with the end time of the first interval (after sorting).
    -   Initialize `count` to 1, as the first interval is always part of our non-overlapping set.
    -   Iterate through the sorted `intervals` starting from the second interval (`i = 1`).
    -   For each `current_interval` (`intervals[i]`):
        -   If `current_interval[0]` (its start time) is greater than or equal to `end` (the end time of the last chosen non-overlapping interval), it means the `current_interval` does not overlap with the previously chosen one. We can include it in our non-overlapping set.
        -   If included, update `end` to `current_interval[1]` and increment `count`.

3.  **Calculate Removals:** The `count` variable now holds the maximum number of non-overlapping intervals. To find the minimum number of intervals to remove, we subtract this `count` from the total number of intervals (`len(intervals) - count`).

**Time and Space Complexity:**

-   **Time Complexity:** O(N log N), primarily due to the sorting step, where N is the number of intervals. The iteration and checking part takes O(N).
-   **Space Complexity:** O(1) if the sorting is done in-place, or O(N) if a new sorted list is created (depending on the sorting algorithm implementation).
