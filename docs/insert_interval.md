
# 72. Insert Interval

## Problem Statement

You are given an array of non-overlapping intervals `intervals` where `intervals[i] = [starti, endi]` represent the start and the end of the `i`th interval and `intervals` is sorted in ascending order by `starti`.

You are also given an interval `newInterval = [start, end]`.

Insert `newInterval` into `intervals` such that `intervals` is still sorted in ascending order by `starti` and `intervals` still does not have any overlapping intervals (merge overlapping intervals if necessary).

Return `intervals` *after the insertion and merging processes*.

**Example 1:**

**Input:** intervals = [[1,3],[6,9]], newInterval = [2,5]
**Output:** [[1,5],[6,9]]

**Example 2:**

**Input:** intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
**Output:** [[1,2],[3,10],[12,16]]
**Explanation:** Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].

**Example 3:**

**Input:** intervals = [], newInterval = [5,7]
**Output:** [[5,7]]

## Solution

```python
class Solution:
    def insert(self, intervals: list[list[int]], newInterval: list[int]) -> list[list[int]]:
        result = []
        i = 0
        n = len(intervals)

        # Add all intervals that come before newInterval and do not overlap
        while i < n and intervals[i][1] < newInterval[0]:
            result.append(intervals[i])
            i += 1

        # Merge overlapping intervals
        while i < n and intervals[i][0] <= newInterval[1]:
            newInterval[0] = min(newInterval[0], intervals[i][0])
            newInterval[1] = max(newInterval[1], intervals[i][1])
            i += 1
        result.append(newInterval)

        # Add all remaining intervals that come after newInterval and do not overlap
        while i < n:
            result.append(intervals[i])
            i += 1

        return result
```

## Explanation

This problem involves inserting a new interval into a sorted list of non-overlapping intervals and then merging any overlaps. Since the input `intervals` list is already sorted, we can use a linear scan approach.

**Core Idea:** We can divide the existing intervals into three categories relative to the `newInterval`:

1.  Intervals that come completely *before* `newInterval` and do not overlap.
2.  Intervals that *overlap* with `newInterval`.
3.  Intervals that come completely *after* `newInterval` and do not overlap.

**Algorithm:**

1.  **Add Non-Overlapping Intervals (Before):**
    -   Iterate through `intervals` from the beginning.
    -   Append any interval `intervals[i]` to `result` if its `end` time (`intervals[i][1]`) is less than the `start` time of `newInterval` (`newInterval[0]`). This means `intervals[i]` is completely before `newInterval` and does not overlap.

2.  **Merge Overlapping Intervals:**
    -   Continue iterating. Now, we encounter intervals that either overlap with `newInterval` or come after it.
    -   While `intervals[i]`'s `start` time (`intervals[i][0]`) is less than or equal to `newInterval`'s `end` time (`newInterval[1]`), it means there is an overlap.
    -   During this overlap, we update `newInterval` by taking the minimum of the `start` times and the maximum of the `end` times to encompass all overlapping intervals.
    -   Increment `i` to move to the next interval.
    -   After this loop, `newInterval` will have been expanded to cover all intervals it overlapped with. Append this updated `newInterval` to `result`.

3.  **Add Non-Overlapping Intervals (After):**
    -   Append all remaining intervals from `intervals` (from the current `i` to the end) to `result`. These intervals are guaranteed not to overlap with the (now merged) `newInterval`.

**Time and Space Complexity:**

-   **Time Complexity:** O(N), where N is the number of intervals. We iterate through the list of intervals at most once.
-   **Space Complexity:** O(N) for the `result` list. In the worst case, no intervals overlap, and `result` will contain all N intervals plus the `newInterval`.
