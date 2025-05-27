[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3559\. Number of Ways to Assign Edge Weights II

Hard

There is an undirected tree with `n` nodes labeled from 1 to `n`, rooted at node 1. The tree is represented by a 2D integer array `edges` of length `n - 1`, where <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> indicates that there is an edge between nodes <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code>.

Initially, all edges have a weight of 0. You must assign each edge a weight of either **1** or **2**.

The **cost** of a path between any two nodes `u` and `v` is the total weight of all edges in the path connecting them.

You are given a 2D integer array `queries`. For each <code>queries[i] = [u<sub>i</sub>, v<sub>i</sub>]</code>, determine the number of ways to assign weights to edges **in the path** such that the cost of the path between <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code> is **odd**.

Return an array `answer`, where `answer[i]` is the number of valid assignments for `queries[i]`.

Since the answer may be large, apply **modulo** <code>10<sup>9</sup> + 7</code> to each `answer[i]`.

**Note:** For each query, disregard all edges **not** in the path between node <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code>.

**Example 1:**

![](https://assets.leetcode.com/uploads/2025/03/23/screenshot-2025-03-24-at-060006.png)

**Input:** edges = \[\[1,2]], queries = \[\[1,1],[1,2]]

**Output:** [0,1]

**Explanation:**

*   Query `[1,1]`: The path from Node 1 to itself consists of no edges, so the cost is 0. Thus, the number of valid assignments is 0.
*   Query `[1,2]`: The path from Node 1 to Node 2 consists of one edge (`1 → 2`). Assigning weight 1 makes the cost odd, while 2 makes it even. Thus, the number of valid assignments is 1.

**Example 2:**

![](https://assets.leetcode.com/uploads/2025/03/23/screenshot-2025-03-24-at-055820.png)

**Input:** edges = \[\[1,2],[1,3],[3,4],[3,5]], queries = \[\[1,4],[3,4],[2,5]]

**Output:** [2,1,4]

**Explanation:**

*   Query `[1,4]`: The path from Node 1 to Node 4 consists of two edges (`1 → 3` and `3 → 4`). Assigning weights (1,2) or (2,1) results in an odd cost. Thus, the number of valid assignments is 2.
*   Query `[3,4]`: The path from Node 3 to Node 4 consists of one edge (`3 → 4`). Assigning weight 1 makes the cost odd, while 2 makes it even. Thus, the number of valid assignments is 1.
*   Query `[2,5]`: The path from Node 2 to Node 5 consists of three edges (`2 → 1, 1 → 3`, and `3 → 5`). Assigning (1,2,2), (2,1,2), (2,2,1), or (1,1,1) makes the cost odd. Thus, the number of valid assignments is 4.

**Constraints:**

*   <code>2 <= n <= 10<sup>5</sup></code>
*   `edges.length == n - 1`
*   <code>edges[i] == [u<sub>i</sub>, v<sub>i</sub>]</code>
*   <code>1 <= queries.length <= 10<sup>5</sup></code>
*   <code>queries[i] == [u<sub>i</sub>, v<sub>i</sub>]</code>
*   <code>1 <= u<sub>i</sub>, v<sub>i</sub> <= n</code>
*   `edges` represents a valid tree.

## Solution

```kotlin
import kotlin.math.ceil
import kotlin.math.ln

class Solution {
    private lateinit var adj: MutableList<MutableList<Int>>
    private lateinit var level: IntArray
    private lateinit var jumps: Array<IntArray?>

    private fun mark(node: Int, par: Int) {
        for (neigh in adj[node]) {
            if (neigh == par) {
                continue
            }
            level[neigh] = level[node] + 1
            jumps[neigh]!![0] = node
            mark(neigh, node)
        }
    }

    fun lift(u: Int, diff: Int): Int {
        var u = u
        var diff = diff
        while (diff > 0) {
            val rightmost = diff xor (diff and (diff - 1))
            val jump = (ln(rightmost.toDouble()) / ln(2.0)).toInt()
            u = jumps[u]!![jump]
            diff -= rightmost
        }
        return u
    }

    private fun findLca(u: Int, v: Int): Int {
        var u = u
        var v = v
        if (level[u] > level[v]) {
            val temp = u
            u = v
            v = temp
        }
        v = lift(v, level[v] - level[u])
        if (u == v) {
            return u
        }
        for (i in jumps[0]!!.indices.reversed()) {
            if (jumps[u]!![i] != jumps[v]!![i]) {
                u = jumps[u]!![i]
                v = jumps[v]!![i]
            }
        }
        return jumps[u]!![0]
    }

    private fun findDist(a: Int, b: Int): Int {
        return level[a] + level[b] - 2 * level[findLca(a, b)]
    }

    fun assignEdgeWeights(edges: Array<IntArray>, queries: Array<IntArray>): IntArray {
        val n = edges.size + 1
        adj = ArrayList<MutableList<Int>>()
        level = IntArray(n)
        for (i in 0..<n) {
            adj.add(ArrayList<Int>())
        }
        for (i in edges) {
            adj[i[0] - 1].add(i[1] - 1)
            adj[i[1] - 1].add(i[0] - 1)
        }
        val m = (ceil(ln(n - 1.0) / ln(2.0))).toInt() + 1
        jumps = Array<IntArray?>(n) { IntArray(m) }
        mark(0, -1)
        for (j in 1..<m) {
            for (i in 0..<n) {
                val p = jumps[i]!![j - 1]
                jumps[i]!![j] = jumps[p]!![j - 1]
            }
        }
        val pow = IntArray(n + 1)
        pow[0] = 1
        for (i in 1..n) {
            pow[i] = (pow[i - 1] * 2) % MOD
        }
        val q = queries.size
        val ans = IntArray(q)
        for (i in 0..<q) {
            val d = findDist(queries[i][0] - 1, queries[i][1] - 1)
            ans[i] = if (d > 0) pow[d - 1] else 0
        }
        return ans
    }

    companion object {
        private const val MOD = 1000000007
    }
}
```