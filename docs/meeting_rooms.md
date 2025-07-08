
# 73. Meeting Rooms

## Problem Statement

Given an array of meeting time intervals `intervals` where `intervals[i] = [starti, endi]`, determine if a person could attend all meetings.

**Example 1:**

**Input:** intervals = [[0,30],[5,10],[15,20]]
**Output:** false

**Example 2:**

**Input:** intervals = [[7,10],[2,4]]
**Output:** true

## Solution

```python
class Solution:
    def canAttendMeetings(self, intervals: list[list[int]]) -> bool:
        # Sort the intervals by their start times
        intervals.sort(key=lambda x: x[0])

        # Iterate through the sorted intervals and check for overlaps
        for i in range(1, len(intervals)):
            # If the current meeting starts before the previous meeting ends,
            # there is an overlap.
            if intervals[i][0] < intervals[i-1][1]:
                return False

        return True
```

## Explanation

This problem asks whether a person can attend all meetings, which means we need to check if any two meetings overlap.

**Core Idea:** If we sort the meetings by their start times, then we only need to check if a meeting overlaps with the *immediately preceding* meeting. If `meeting[i]` overlaps with `meeting[i-1]`, then it's impossible to attend all meetings.

1.  **Sort Intervals:** The first and most crucial step is to sort the `intervals` list based on the `start` times of the meetings. This ensures that we process meetings in chronological order.

2.  **Check for Overlaps:**
    -   Iterate through the sorted `intervals` list, starting from the second meeting (`i = 1`).
    -   For each `intervals[i]`, compare its `start` time (`intervals[i][0]`) with the `end` time of the previous meeting (`intervals[i-1][1]`).
    -   If `intervals[i][0] < intervals[i-1][1]`, it means the current meeting starts before the previous one ends, indicating an overlap. In this case, the person cannot attend all meetings, so we immediately return `False`.

3.  **Return True:** If the loop completes without finding any overlaps, it means all meetings can be attended, so we return `True`.

**Time and Space Complexity:**

-   **Time Complexity:** O(N log N), primarily due to the sorting step, where N is the number of meetings. The iteration and checking part takes O(N).
-   **Space Complexity:** O(1) if the sorting is done in-place, or O(N) if a new sorted list is created (depending on the sorting algorithm implementation).
