
# 47. Merge K Sorted Lists

## Problem Statement

You are given an array of `k` linked-lists `lists`, each linked list is sorted in ascending order.

Merge all the linked-lists into one sorted linked list and return it.

**Example 1:**

**Input:** lists = [[1,4,5],[1,3,4],[2,6]]
**Output:** [1,1,2,3,4,4,5,6]
**Explanation:** The linked lists are:
[
  1->4->5,
  1->3->4,
  2->6
]
merging them into one sorted list:
1->1->2->3->4->4->5->6

**Example 2:**

**Input:** lists = []
**Output:** []

**Example 3:**

**Input:** lists = [[]]
**Output:** []

## Solution

```python
import heapq

# Definition for singly-linked list.
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def mergeKLists(self, lists: list[ListNode]) -> ListNode:
        # Use a min-heap to keep track of the smallest element from each list
        min_heap = []

        # Push the head of each non-empty list into the heap
        # The heap stores tuples: (value, list_index, node_object)
        # We need list_index to break ties if values are equal, though not strictly necessary for correctness
        # but good practice for heapq with custom objects.
        for i, l in enumerate(lists):
            if l:
                heapq.heappush(min_heap, (l.val, i, l))

        # Create a dummy node to build the merged list
        dummy = ListNode()
        current = dummy

        while min_heap:
            # Pop the smallest element from the heap
            val, list_idx, node = heapq.heappop(min_heap)

            # Append it to the merged list
            current.next = node
            current = current.next

            # If the popped node has a next node, push it to the heap
            if node.next:
                heapq.heappush(min_heap, (node.next.val, list_idx, node.next))

        return dummy.next
```

## Explanation

This problem can be efficiently solved using a min-priority queue (min-heap).

**Core Idea:** At any point, the next node to be added to our merged list must be the smallest among the current heads of all `k` linked lists. A min-heap is perfect for this, as it always gives us the minimum element in O(log k) time.

1.  **Initialize Min-Heap:** Create an empty min-heap.

2.  **Populate Heap with Initial Nodes:** Iterate through the `k` linked lists. For each non-empty list, push its head node into the min-heap. The heap will store tuples of `(node.val, list_index, node_object)`. The `list_index` is used to break ties if two nodes have the same value, which is important for `heapq` when comparing custom objects.

3.  **Build Merged List:**
    -   Create a `dummy` node and a `current` pointer to build the merged list.
    -   While the `min_heap` is not empty:
        -   Extract the node with the smallest value from the heap (`heapq.heappop`).
        -   Append this node to `current.next` and advance `current`.
        -   If the extracted node has a `next` node, push that `next` node into the heap. This ensures that we always have the next available smallest element from that particular list.

4.  **Return Result:** The merged sorted list starts from `dummy.next`.

**Time and Space Complexity:**

-   **Time Complexity:** O(N log k), where N is the total number of nodes across all linked lists, and k is the number of linked lists. Each node is pushed and popped from the heap exactly once. Each heap operation takes O(log k) time.
-   **Space Complexity:** O(k) for the min-heap, as it will store at most `k` nodes (one from each list) at any given time.
