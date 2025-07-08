
# 28. Course Schedule

## Problem Statement

There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you **must** take course `bi` first if you want to take course `ai`.

- For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.

Return `true` if you can finish all courses. Otherwise, return `false`.

**Example 1:**

**Input:** numCourses = 2, prerequisites = [[1,0]]
**Output:** true
**Explanation:** There are a total of 2 courses to take. To take course 1 you should have finished course 0. So it is possible.

**Example 2:**

**Input:** numCourses = 2, prerequisites = [[1,0],[0,1]]
**Output:** false
**Explanation:** There are a total of 2 courses to take. To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.

## Solution

```python
class Solution:
    def canFinish(self, numCourses: int, prerequisites: list[list[int]]) -> bool:
        # Build adjacency list
        adj = {i: [] for i in range(numCourses)}
        for course, prereq in prerequisites:
            adj[course].append(prereq)

        # A set to keep track of nodes in the current recursion stack (visiting)
        visiting = set()

        # A set to keep track of nodes that have been fully processed
        visited = set()

        def has_cycle(course):
            visiting.add(course)

            for prereq in adj[course]:
                if prereq in visiting:
                    # Cycle detected
                    return True
                if prereq not in visited:
                    if has_cycle(prereq):
                        return True

            # No cycle found from this node, move it from visiting to visited
            visiting.remove(course)
            visited.add(course)
            return False

        # Check for cycles from every course
        for course in range(numCourses):
            if course not in visited:
                if has_cycle(course):
                    return False

        return True
```

## Explanation

This problem is equivalent to detecting a cycle in a directed graph. The courses are the nodes, and the prerequisites are the directed edges.

If there is a cycle in the graph (e.g., Course A depends on B, B depends on C, and C depends on A), then it's impossible to finish all courses. If there are no cycles, it's always possible.

We can solve this using Depth-First Search (DFS).

1.  **Build Adjacency List:** First, we represent the graph using an adjacency list, where `adj[course]` contains a list of its prerequisites.

2.  **Track Visited States:** We need to keep track of the state of each node during the DFS traversal. We use two sets:
    -   `visiting`: Stores nodes that are currently in the recursion stack for the active DFS path. If we encounter a node that is already in `visiting`, we have found a back edge, which means there is a cycle.
    -   `visited`: Stores nodes that have been completely explored (i.e., all their neighbors have been visited) and are known to not be part of a cycle.

3.  **DFS with Cycle Detection:**
    -   The `has_cycle` function performs the DFS.
    -   When we visit a `course`, we add it to `visiting`.
    -   We then recursively call `has_cycle` for all its `prereq`s.
    -   If any `prereq` is already in `visiting`, we've found a cycle, and we return `True`.
    -   After visiting all neighbors of a `course` without finding a cycle, we remove it from `visiting` and add it to `visited`. This is an important optimization; it means we don't have to re-explore this node if we encounter it again from a different path.

4.  **Main Loop:** We iterate through all courses from `0` to `numCourses - 1`. We start a DFS from each course that hasn't been `visited` yet to ensure we cover all components of the graph.

The time complexity is O(V + E), where V is the number of courses (vertices) and E is the number of prerequisites (edges), because we visit each vertex and edge once. The space complexity is O(V + E) for the adjacency list and the visited sets.
