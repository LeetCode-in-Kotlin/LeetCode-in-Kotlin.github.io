[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1786\. Number of Restricted Paths From First to Last Node

Medium

There is an undirected weighted connected graph. You are given a positive integer `n` which denotes that the graph has `n` nodes labeled from `1` to `n`, and an array `edges` where each <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, weight<sub>i</sub>]</code> denotes that there is an edge between nodes <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code> with weight equal to <code>weight<sub>i</sub></code>.

A path from node `start` to node `end` is a sequence of nodes <code>[z<sub>0</sub>, z<sub>1</sub>, z<sub>2</sub>, ..., z<sub>k</sub>]</code> such that <code>z<sub>0</sub> = start</code> and <code>z<sub>k</sub> = end</code> and there is an edge between <code>z<sub>i</sub></code> and <code>z<sub>i+1</sub></code> where `0 <= i <= k-1`.

The distance of a path is the sum of the weights on the edges of the path. Let `distanceToLastNode(x)` denote the shortest distance of a path between node `n` and node `x`. A **restricted path** is a path that also satisfies that <code>distanceToLastNode(z<sub>i</sub>) > distanceToLastNode(z<sub>i+1</sub>)</code> where `0 <= i <= k-1`.

Return _the number of restricted paths from node_ `1` _to node_ `n`. Since that number may be too large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/17/restricted_paths_ex1.png)

**Input:** n = 5, edges = \[\[1,2,3],[1,3,3],[2,3,1],[1,4,2],[5,2,2],[3,5,1],[5,4,10]]

**Output:** 3

**Explanation:** Each circle contains the node number in black and its `distanceToLastNode value in blue.` The three restricted paths are:

1) 1 --> 2 --> 5

2) 1 --> 2 --> 3 --> 5

3) 1 --> 3 --> 5 

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/02/17/restricted_paths_ex22.png)

**Input:** n = 7, edges = \[\[1,3,1],[4,1,2],[7,3,4],[2,5,3],[5,6,1],[6,7,2],[7,5,3],[2,6,4]]

**Output:** 1

**Explanation:** Each circle contains the node number in black and its `distanceToLastNode value in blue.` The only restricted path is 1 --> 3 --> 7. 

**Constraints:**

*   <code>1 <= n <= 2 * 10<sup>4</sup></code>
*   <code>n - 1 <= edges.length <= 4 * 10<sup>4</sup></code>
*   `edges[i].length == 3`
*   <code>1 <= u<sub>i</sub>, v<sub>i</sub> <= n</code>
*   <code>u<sub>i</sub> != v<sub>i</sub></code>
*   <code>1 <= weight<sub>i</sub> <= 10<sup>5</sup></code>
*   There is at most one edge between any two nodes.
*   There is at least one path between any two nodes.

## Solution

```kotlin
import java.util.PriorityQueue

class Solution {
    private class Pair(var v: Int, var cwt: Int) : Comparable<Pair> {
        override fun compareTo(other: Pair): Int {
            return cwt - other.cwt
        }
    }

    private class Edge(var v: Int, var wt: Int)

    private lateinit var dtl: IntArray
    private lateinit var dp: IntArray

    fun countRestrictedPaths(n: Int, edges: Array<IntArray>): Int {
        val graph = buildGraph(n, edges)
        val pq = PriorityQueue<Pair>()
        val vis = BooleanArray(n + 1)
        dtl = IntArray(n + 1)
        pq.add(Pair(n, 0))
        while (pq.isNotEmpty()) {
            val rem = pq.remove()
            if (vis[rem.v]) {
                continue
            }
            dtl[rem.v] = rem.cwt
            vis[rem.v] = true
            for (edge in graph[rem.v]) {
                if (!vis[edge.v]) {
                    pq.add(Pair(edge.v, rem.cwt + edge.wt))
                }
            }
        }
        dp = IntArray(n + 1)
        return dfs(graph, 1, BooleanArray(n + 1), n)
    }

    private fun dfs(graph: List<MutableList<Edge>>, vtx: Int, vis: BooleanArray, n: Int): Int {
        if (vtx == n) {
            return 1
        }
        var ans: Long = 0
        vis[vtx] = true
        for (edge in graph[vtx]) {
            if (!vis[edge.v] && dtl[edge.v] < dtl[vtx]) {
                val x = dfs(graph, edge.v, vis, n)
                ans = (ans + x) % M
            } else if (dtl[edge.v] < dtl[vtx] && vis[edge.v]) {
                ans = (ans + dp[edge.v]) % M
            }
        }
        dp[vtx] = ans.toInt()
        return ans.toInt()
    }

    private fun buildGraph(n: Int, edges: Array<IntArray>): List<MutableList<Edge>> {
        val graph: MutableList<MutableList<Edge>> = ArrayList()
        for (i in 0..n) {
            graph.add(ArrayList())
        }
        for (edge in edges) {
            val u = edge[0]
            val v = edge[1]
            val wt = edge[2]
            graph[u].add(Edge(v, wt))
            graph[v].add(Edge(u, wt))
        }
        return graph
    }

    companion object {
        private const val M = 1000000007
    }
}
```