[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 684\. Redundant Connection

Medium

In this problem, a tree is an **undirected graph** that is connected and has no cycles.

You are given a graph that started as a tree with `n` nodes labeled from `1` to `n`, with one additional edge added. The added edge has two **different** vertices chosen from `1` to `n`, and was not an edge that already existed. The graph is represented as an array `edges` of length `n` where <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> indicates that there is an edge between nodes <code>a<sub>i</sub></code> and <code>b<sub>i</sub></code> in the graph.

Return _an edge that can be removed so that the resulting graph is a tree of_ `n` _nodes_. If there are multiple answers, return the answer that occurs last in the input.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/05/02/reduntant1-1-graph.jpg)

**Input:** edges = \[\[1,2],[1,3],[2,3]]

**Output:** [2,3]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/05/02/reduntant1-2-graph.jpg)

**Input:** edges = \[\[1,2],[2,3],[3,4],[1,4],[1,5]]

**Output:** [1,4]

**Constraints:**

*   `n == edges.length`
*   `3 <= n <= 1000`
*   `edges[i].length == 2`
*   <code>1 <= a<sub>i</sub> < b<sub>i</sub> <= edges.length</code>
*   <code>a<sub>i</sub> != b<sub>i</sub></code>
*   There are no repeated edges.
*   The given graph is connected.

## Solution

```kotlin
class Solution {
    private lateinit var par: IntArray
    fun findRedundantConnection(edges: Array<IntArray>): IntArray {
        val ans = IntArray(2)
        val n = edges.size
        par = IntArray(n + 1)
        for (i in 0 until n) {
            par[i] = i
        }
        for (edge in edges) {
            val lx = find(edge[0])
            val ly = find(edge[1])
            if (lx != ly) {
                par[lx] = ly
            } else {
                ans[0] = edge[0]
                ans[1] = edge[1]
            }
        }
        return ans
    }

    private fun find(x: Int): Int {
        return if (par[x] == x) {
            x
        } else find(par[x])
    }
}
```