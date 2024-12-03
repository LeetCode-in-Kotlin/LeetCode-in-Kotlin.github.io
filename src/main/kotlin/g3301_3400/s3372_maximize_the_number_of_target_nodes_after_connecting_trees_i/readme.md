[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3372\. Maximize the Number of Target Nodes After Connecting Trees I

Medium

There exist two **undirected** trees with `n` and `m` nodes, with **distinct** labels in ranges `[0, n - 1]` and `[0, m - 1]`, respectively.

You are given two 2D integer arrays `edges1` and `edges2` of lengths `n - 1` and `m - 1`, respectively, where <code>edges1[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> indicates that there is an edge between nodes <code>a<sub>i</sub></code> and <code>b<sub>i</sub></code> in the first tree and <code>edges2[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> indicates that there is an edge between nodes <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code> in the second tree. You are also given an integer `k`.

Node `u` is **target** to node `v` if the number of edges on the path from `u` to `v` is less than or equal to `k`. **Note** that a node is _always_ **target** to itself.

Return an array of `n` integers `answer`, where `answer[i]` is the **maximum** possible number of nodes **target** to node `i` of the first tree if you have to connect one node from the first tree to another node in the second tree.

**Note** that queries are independent from each other. That is, for every query you will remove the added edge before proceeding to the next query.

**Example 1:**

**Input:** edges1 = \[\[0,1],[0,2],[2,3],[2,4]], edges2 = \[\[0,1],[0,2],[0,3],[2,7],[1,4],[4,5],[4,6]], k = 2

**Output:** [9,7,9,8,8]

**Explanation:**

*   For `i = 0`, connect node 0 from the first tree to node 0 from the second tree.
*   For `i = 1`, connect node 1 from the first tree to node 0 from the second tree.
*   For `i = 2`, connect node 2 from the first tree to node 4 from the second tree.
*   For `i = 3`, connect node 3 from the first tree to node 4 from the second tree.
*   For `i = 4`, connect node 4 from the first tree to node 4 from the second tree.

![](https://assets.leetcode.com/uploads/2024/09/24/3982-1.png)

**Example 2:**

**Input:** edges1 = \[\[0,1],[0,2],[0,3],[0,4]], edges2 = \[\[0,1],[1,2],[2,3]], k = 1

**Output:** [6,3,3,3,3]

**Explanation:**

For every `i`, connect node `i` of the first tree with any node of the second tree.

![](https://assets.leetcode.com/uploads/2024/09/24/3928-2.png)

**Constraints:**

*   `2 <= n, m <= 1000`
*   `edges1.length == n - 1`
*   `edges2.length == m - 1`
*   `edges1[i].length == edges2[i].length == 2`
*   <code>edges1[i] = [a<sub>i</sub>, b<sub>i</sub>]</code>
*   <code>0 <= a<sub>i</sub>, b<sub>i</sub> < n</code>
*   <code>edges2[i] = [u<sub>i</sub>, v<sub>i</sub>]</code>
*   <code>0 <= u<sub>i</sub>, v<sub>i</sub> < m</code>
*   The input is generated such that `edges1` and `edges2` represent valid trees.
*   `0 <= k <= 1000`

## Solution

```kotlin
import kotlin.math.max

class Solution {
    private fun getGraph(edges: Array<IntArray>): Array<ArrayList<Int?>?> {
        val n = edges.size + 1
        val graph: Array<ArrayList<Int?>?> = arrayOfNulls<ArrayList<Int?>?>(n)
        for (i in 0..<n) {
            graph[i] = ArrayList<Int?>()
        }
        for (edge in edges) {
            val u = edge[0]
            val v = edge[1]
            graph[u]!!.add(v)
            graph[v]!!.add(u)
        }
        return graph
    }

    private fun dfs(graph: Array<ArrayList<Int?>?>, u: Int, pt: Int, dp: Array<IntArray?>, k: Int) {
        for (v in graph[u]!!) {
            if (v == pt) {
                continue
            }
            dfs(graph, v!!, u, dp, k)
            for (i in 0..<k) {
                dp[u]!![i + 1] += dp[v]!![i]
            }
        }
        dp[u]!![0] = dp[u]!![0] + 1
    }

    private fun dfs2(
        graph: Array<ArrayList<Int?>?>,
        u: Int,
        pt: Int,
        ptv: IntArray,
        fdp: Array<IntArray?>,
        dp: Array<IntArray?>,
        k: Int,
    ) {
        fdp[u]!![0] = dp[u]!![0]
        for (i in 1..k) {
            fdp[u]!![i] = (dp[u]!![i] + ptv[i - 1])
        }
        for (v in graph[u]!!) {
            if (v == pt) {
                continue
            }
            val nptv = IntArray(k + 1)
            for (i in 0..<k) {
                nptv[i + 1] = dp[u]!![i + 1] - dp[v!!]!![i] + ptv[i]
            }
            nptv[0] = 1
            dfs2(graph, v!!, u, nptv, fdp, dp, k)
        }
    }

    private fun get(edges: Array<IntArray>, k: Int): Array<IntArray?> {
        val graph = getGraph(edges)
        val n = graph.size
        val dp = Array<IntArray?>(n) { IntArray(k + 1) }
        val fdp = Array<IntArray?>(n) { IntArray(k + 1) }
        dfs(graph, 0, -1, dp, k)
        dfs2(graph, 0, -1, IntArray(k + 1), fdp, dp, k)
        for (i in 0..<n) {
            for (j in 1..k) {
                fdp[i]!![j] += fdp[i]!![j - 1]
            }
        }
        return fdp
    }

    fun maxTargetNodes(edges1: Array<IntArray>, edges2: Array<IntArray>, k: Int): IntArray {
        val a = get(edges1, k)
        val b = get(edges2, k)
        val n = a.size
        val m = b.size
        val ans = IntArray(n)
        var max = 0
        run {
            var i = 0
            while (k != 0 && i < m) {
                max = max(max.toDouble(), b[i]!![k - 1].toDouble()).toInt()
                i++
            }
        }
        for (i in 0..<n) {
            ans[i] = a[i]!![k] + max
        }
        return ans
    }
}
```