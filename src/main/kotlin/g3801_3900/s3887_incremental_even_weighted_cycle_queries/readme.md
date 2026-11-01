[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3887\. Incremental Even-Weighted Cycle Queries

Hard

You are given a positive integer `n`.

There is an **undirected** graph with `n` nodes labeled from 0 to `n - 1`. Initially, the graph has no edges.

You are also given a 2D integer array `edges`, where <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code> represents an edge between nodes <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code> with weight <code>w<sub>i</sub></code>. The weight <code>w<sub>i</sub></code> is either 0 or 1.

Process the edges in `edges` in the given order. For each edge, add it to the graph only if, after adding it, the sum of the weights of the edges in **every** cycle in the resulting graph is **even**.

Return an integer denoting the number of edges that are successfully added to the graph.

**Example 1:**

**Input:** n = 3, edges = \[\[0,1,1],[1,2,1],[0,2,1]]

**Output:** 2

**Explanation:**

![](https://assets.leetcode.com/uploads/2026/03/21/hmadizgovu.png)

*   `[0, 1, 1]`: We add the edge between vertex 0 and vertex 1 with weight 1.
*   `[1, 2, 1]`: We add the edge between vertex 1 and vertex 2 with weight 1.
*   `[0, 2, 1]`: The edge between vertex 0 and vertex 2 (the dashed edge in the diagram) is not added because the cycle `0 - 1 - 2 - 0` has total edge weight `1 + 1 + 1 = 3`, which is an odd number.

**Example 2:**

**Input:** n = 3, edges = \[\[0,1,1],[1,2,1],[0,2,0]]

**Output:** 3

**Explanation:**

![](https://assets.leetcode.com/uploads/2026/03/21/rbdgrefwok.png)

*   `[0, 1, 1]`: We add the edge between vertex 0 and vertex 1 with weight 1.
*   `[1, 2, 1]`: We add the edge between vertex 1 and vertex 2 with weight 1.
*   `[0, 2, 0]`: We add the edge between vertex 0 and vertex 2 with weight 0.
*   Note that the cycle `0 - 1 - 2 - 0` has total edge weight `1 + 1 + 0 = 2`, which is an even number.

**Constraints:**

*   <code>3 <= n <= 5 * 10<sup>4</sup></code>
*   <code>1 <= edges.length <= 5 * 10<sup>4</sup></code>
*   <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code>
*   <code>0 <= u<sub>i</sub> < v<sub>i</sub> < n</code>
*   All edges are distinct.
*   <code>w<sub>i</sub> = 0 or w<sub>i</sub> = 1</code>

## Solution

```kotlin
class Solution {
    private lateinit var parent: IntArray
    private lateinit var parity: IntArray

    private fun find(x: Int): Int {
        if (parent[x] == x) {
            return x
        }
        val p = parent[x]
        parent[x] = find(parent[x])
        parity[x] = parity[x] xor parity[p]
        return parent[x]
    }

    fun numberOfEdgesAdded(n: Int, edges: Array<IntArray>): Int {
        parent = IntArray(n)
        val rank = IntArray(n)
        parity = IntArray(n)
        for (i in 0..<n) {
            parent[i] = i
        }
        var ans = 0
        for (e in edges) {
            val u = e[0]
            val v = e[1]
            val w = e[2]
            var ru = find(u)
            var rv = find(v)
            var pu = parity[u]
            var pv = parity[v]
            if (ru == rv) {
                if ((pu xor pv) == w) {
                    ans++
                }
            } else {
                if (rank[ru] < rank[rv]) {
                    var temp = ru
                    ru = rv
                    rv = temp
                    temp = pu
                    pu = pv
                    pv = temp
                }
                parent[rv] = ru
                parity[rv] = pu xor pv xor w
                if (rank[ru] == rank[rv]) {
                    rank[ru]++
                }
                ans++
            }
        }
        return ans
    }
}
```