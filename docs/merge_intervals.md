
# 71. Merge Intervals

## Problem Statement

Given an array of `intervals` where `intervals[i] = [starti, endi]`, merge all overlapping intervals, and return *an array of the non-overlapping intervals that cover all the intervals in the input*.

**Example 1:**

**Input:** intervals = [[1,3],[2,6],[8,10],[15,18]]
**Output:** [[1,6],[8,10],[15,18]]
**Explanation:** Since intervals [1,3] and [2,6] overlap, merge them into [1,6].

**Example 2:**

**Input:** intervals = [[1,4],[4,5]]
**Output:** [[1,5]]
**Explanation:** Intervals [1,4] and [4,5] are considered overlapping.

## Solution

```python
class Solution:
    def merge(self, intervals: list[list[int]]) -> list[list[int]]:
        if not intervals:
            return []

        # Sort intervals based on their start times
        intervals.sort(key=lambda x: x[0])

        merged = []
        for interval in intervals:
            # If the merged list is empty or the current interval does not overlap
            # with the previous merged interval, simply add it.
            if not merged or interval[0] > merged[-1][1]:
                merged.append(interval)
            else:
                # Otherwise, there is an overlap, so merge the current and previous intervals
                # by updating the end time of the last merged interval.
                merged[-1][1] = max(merged[-1][1], interval[1])

        return merged
```

## Explanation

This problem can be solved efficiently by first sorting the intervals and then iterating through them to merge overlapping ones.

**Core Idea:** If intervals are sorted by their start times, then any overlapping intervals must be adjacent or partially overlapping in the sorted list.

1.  **Sort Intervals:** The first crucial step is to sort the input `intervals` based on their `start` times. This ensures that we process intervals in a sequential order.

2.  **Iterate and Merge:**
    -   Initialize an empty list `merged` to store the non-overlapping intervals.
    -   Iterate through each `interval` in the sorted `intervals` list.
    -   **Check for Overlap:**
        -   If `merged` is empty, or if the current `interval`'s `start` time is greater than the `end` time of the *last* interval in `merged`, it means there is no overlap. In this case, simply append the current `interval` to `merged`.
        -   If there is an overlap (i.e., `interval[0] <= merged[-1][1]`), it means the current interval can be merged with the last interval in `merged`. To merge, we update the `end` time of the last interval in `merged` to be the maximum of its current `end` time and the current `interval`'s `end` time (`merged[-1][1] = max(merged[-1][1], interval[1])`).

3.  **Return Result:** After iterating through all intervals, `merged` will contain the non-overlapping intervals.

**Time and Space Complexity:**

-   **Time Complexity:** O(N log N), primarily due to the sorting step, where N is the number of intervals. The iteration and merging part takes O(N).
-   **Space Complexity:** O(N) for storing the `merged` list. In the worst case, no intervals overlap, and `merged` will contain all N intervals.
