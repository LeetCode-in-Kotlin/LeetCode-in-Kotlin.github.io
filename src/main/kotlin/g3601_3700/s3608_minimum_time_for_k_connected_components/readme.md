[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3608\. Minimum Time for K Connected Components

Medium

You are given an integer `n` and an undirected graph with `n` nodes labeled from 0 to `n - 1`. This is represented by a 2D array `edges`, where <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, time<sub>i</sub>]</code> indicates an undirected edge between nodes <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code> that can be removed at <code>time<sub>i</sub></code>.

You are also given an integer `k`.

Initially, the graph may be connected or disconnected. Your task is to find the **minimum** time `t` such that after removing all edges with `time <= t`, the graph contains **at least** `k` connected components.

Return the **minimum** time `t`.

A **connected component** is a subgraph of a graph in which there exists a path between any two vertices, and no vertex of the subgraph shares an edge with a vertex outside of the subgraph.

**Example 1:**

**Input:** n = 2, edges = \[\[0,1,3]], k = 2

**Output:** 3

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/05/31/screenshot-2025-06-01-at-022724.png)

*   Initially, there is one connected component `{0, 1}`.
*   At `time = 1` or `2`, the graph remains unchanged.
*   At `time = 3`, edge `[0, 1]` is removed, resulting in `k = 2` connected components `{0}`, `{1}`. Thus, the answer is 3.

**Example 2:**

**Input:** n = 3, edges = \[\[0,1,2],[1,2,4]], k = 3

**Output:** 4

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/05/31/screenshot-2025-06-01-at-022812.png)

*   Initially, there is one connected component `{0, 1, 2}`.
*   At `time = 2`, edge `[0, 1]` is removed, resulting in two connected components `{0}`, `{1, 2}`.
*   At `time = 4`, edge `[1, 2]` is removed, resulting in `k = 3` connected components `{0}`, `{1}`, `{2}`. Thus, the answer is 4.

**Example 3:**

**Input:** n = 3, edges = \[\[0,2,5]], k = 2

**Output:** 0

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/05/31/screenshot-2025-06-01-at-022930.png)

*   Since there are already `k = 2` disconnected components `{1}`, `{0, 2}`, no edge removal is needed. Thus, the answer is 0.

**Constraints:**

*   <code>1 <= n <= 10<sup>5</sup></code>
*   <code>0 <= edges.length <= 10<sup>5</sup></code>
*   <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, time<sub>i</sub>]</code>
*   <code>0 <= u<sub>i</sub>, v<sub>i</sub> < n</code>
*   <code>u<sub>i</sub> != v<sub>i</sub></code>
*   <code>1 <= time<sub>i</sub> <= 10<sup>9</sup></code>
*   `1 <= k <= n`
*   There are no duplicate edges.

## Solution

```kotlin
class Solution {
    fun minTime(n: Int, edges: Array<IntArray>, k: Int): Int {
        var maxTime = 0
        for (e in edges) {
            if (e[2] > maxTime) {
                maxTime = e[2]
            }
        }
        var lo = 0
        var hi = maxTime
        var ans = maxTime
        while (lo <= hi) {
            val mid = lo + (hi - lo) / 2
            if (countComponents(n, edges, mid) >= k) {
                ans = mid
                hi = mid - 1
            } else {
                lo = mid + 1
            }
        }
        return ans
    }

    private fun countComponents(n: Int, edges: Array<IntArray>, t: Int): Int {
        val parent = IntArray(n)
        for (i in 0..<n) {
            parent[i] = i
        }
        var comps = n
        for (e in edges) {
            if (e[2] > t) {
                val u = find(parent, e[0])
                val v = find(parent, e[1])
                if (u != v) {
                    parent[v] = u
                    comps--
                }
            }
        }
        return comps
    }

    private fun find(parent: IntArray, x: Int): Int {
        var x = x
        while (parent[x] != x) {
            parent[x] = parent[parent[x]]
            x = parent[x]
        }
        return x
    }
}
```