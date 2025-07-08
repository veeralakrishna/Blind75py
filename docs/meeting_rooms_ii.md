
# 74. Meeting Rooms II

## Problem Statement

Given an array of meeting time intervals `intervals` where `intervals[i] = [starti, endi]`, return *the minimum number of conference rooms required*.

**Example 1:**

**Input:** intervals = [[0,30],[5,10],[15,20]]
**Output:** 2
**Explanation:**
We need two rooms:
Room 1: (0,30)
Room 2: (5,10), (15,20)

**Example 2:**

**Input:** intervals = [[7,10],[2,4]]
**Output:** 1

## Solution

```python
import heapq

class Solution:
    def minMeetingRooms(self, intervals: list[list[int]]) -> int:
        if not intervals:
            return 0

        # Sort the intervals by their start times
        intervals.sort(key=lambda x: x[0])

        # Min-heap to store the end times of meetings currently in rooms
        # The smallest end time will always be at the top
        rooms = []

        # Add the first meeting's end time to the heap
        heapq.heappush(rooms, intervals[0][1])

        # Process the rest of the meetings
        for i in range(1, len(intervals)):
            # If the current meeting starts at or after the earliest ending meeting,
            # we can reuse that room.
            if intervals[i][0] >= rooms[0]:
                heapq.heappop(rooms) # Remove the earliest ending meeting
            
            # Assign the current meeting to a room (either a new one or a reused one)
            heapq.heappush(rooms, intervals[i][1])

        # The number of rooms in the heap is the minimum number of rooms required
        return len(rooms)
```

## Explanation

This problem asks for the minimum number of meeting rooms needed. This is a classic scheduling problem that can be solved efficiently using a min-heap.

**Core Idea:** We want to reuse rooms as much as possible. When a new meeting starts, we check if any existing meeting has already ended. If so, we can reuse that room. Otherwise, we need a new room.

1.  **Sort Intervals:** First, sort all meeting `intervals` by their `start` times. This ensures we process meetings in the order they begin.

2.  **Min-Heap for Room End Times:** Create a min-heap (`rooms`). This heap will store the `end` times of all meetings that are currently occupying a room. The smallest `end` time will always be at the top of the heap.

3.  **Process Meetings:**
    -   Add the `end` time of the first meeting to the `rooms` heap.
    -   Iterate through the rest of the sorted `intervals`:
        -   For each `current_meeting` (`intervals[i]`):
            -   Check if the `current_meeting`'s `start` time (`intervals[i][0]`) is greater than or equal to the `end` time of the earliest ending meeting in our `rooms` heap (`rooms[0]`).
            -   If it is, it means a room has become free. We can reuse that room, so we `heapq.heappop(rooms)` to remove the earliest ending meeting.
            -   Regardless of whether a room was reused or not, we assign the `current_meeting` to a room by adding its `end` time (`intervals[i][1]`) to the `rooms` heap (`heapq.heappush`). This effectively marks the room as occupied until this meeting ends.

4.  **Result:** After processing all meetings, the number of elements remaining in the `rooms` heap will be the minimum number of conference rooms required. This is because each element in the heap represents a room that is currently occupied.

**Time and Space Complexity:**

-   **Time Complexity:** O(N log N), where N is the number of intervals. The sorting takes O(N log N). Each heap operation (push and pop) takes O(log K) time, where K is the number of rooms (at most N). Since we perform N heap operations, this part is O(N log N).
-   **Space Complexity:** O(N) in the worst case, as the heap could potentially store all N meeting end times if all meetings overlap.
