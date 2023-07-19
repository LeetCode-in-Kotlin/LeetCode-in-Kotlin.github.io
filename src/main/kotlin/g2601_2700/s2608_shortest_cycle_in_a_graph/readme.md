[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2608\. Shortest Cycle in a Graph

Hard

There is a **bi-directional** graph with `n` vertices, where each vertex is labeled from `0` to `n - 1`. The edges in the graph are represented by a given 2D integer array `edges`, where <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> denotes an edge between vertex <code>u<sub>i</sub></code> and vertex <code>v<sub>i</sub></code>. Every vertex pair is connected by at most one edge, and no vertex has an edge to itself.

Return _the length of the **shortest** cycle in the graph_. If no cycle exists, return `-1`.

A cycle is a path that starts and ends at the same node, and each edge in the path is used only once.

**Example 1:**

![](https://assets.leetcode.com/uploads/2023/01/04/cropped.png)

**Input:** n = 7, edges = \[\[0,1],[1,2],[2,0],[3,4],[4,5],[5,6],[6,3]]

**Output:** 3

**Explanation:** The cycle with the smallest length is : 0 -> 1 -> 2 -> 0

**Example 2:**

![](https://assets.leetcode.com/uploads/2023/01/04/croppedagin.png)

**Input:** n = 4, edges = \[\[0,1],[0,2]]

**Output:** -1

**Explanation:** There are no cycles in this graph.

**Constraints:**

*   `2 <= n <= 1000`
*   `1 <= edges.length <= 1000`
*   `edges[i].length == 2`
*   <code>0 <= u<sub>i</sub>, v<sub>i</sub> < n</code>
*   <code>u<sub>i</sub> != v<sub>i</sub></code>
*   There are no repeated edges.

## Solution

```kotlin
import java.util.LinkedList
import java.util.Queue

class Solution {
    private var min = Int.MAX_VALUE
    fun findShortestCycle(n: Int, edges: Array<IntArray>): Int {
        val adj: MutableList<MutableList<Int>> = ArrayList()
        for (i in 0 until n) adj.add(ArrayList())
        for (edge in edges) {
            adj[edge[0]].add(edge[1])
            adj[edge[1]].add(edge[0])
        }
        for (i in 0 until n) {
            dfs(adj, HashSet<Int?>(), i)
        }
        return if (min == Int.MAX_VALUE) -1 else min
    }

    private fun dfs(adj: List<MutableList<Int>>, set: HashSet<Int?>, node: Int) {
        val queue: Queue<IntArray> = LinkedList()
        set.add(node)
        queue.add(intArrayOf(node, node))
        val distance = IntArray(adj.size)
        distance.fill(-1)
        distance[node] = 0
        while (queue.isNotEmpty()) {
            val arr: IntArray = queue.poll()
            val topNode = arr[0]
            val from = arr[1]
            for (i in adj[topNode]) {
                if (i == from) continue
                if (set.contains(i)) {
                    min = min.coerceAtMost(distance[topNode] + distance[i] + 1)
                    continue
                }
                set.add(i)
                distance[i] = distance[topNode] + 1
                queue.add(intArrayOf(i, topNode))
            }
        }
    }
}
```