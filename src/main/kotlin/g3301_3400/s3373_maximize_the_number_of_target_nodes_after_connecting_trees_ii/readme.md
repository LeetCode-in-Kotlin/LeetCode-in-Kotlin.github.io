[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3373\. Maximize the Number of Target Nodes After Connecting Trees II

Hard

There exist two **undirected** trees with `n` and `m` nodes, labeled from `[0, n - 1]` and `[0, m - 1]`, respectively.

You are given two 2D integer arrays `edges1` and `edges2` of lengths `n - 1` and `m - 1`, respectively, where <code>edges1[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> indicates that there is an edge between nodes <code>a<sub>i</sub></code> and <code>b<sub>i</sub></code> in the first tree and <code>edges2[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> indicates that there is an edge between nodes <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code> in the second tree.

Node `u` is **target** to node `v` if the number of edges on the path from `u` to `v` is even. **Note** that a node is _always_ **target** to itself.

Return an array of `n` integers `answer`, where `answer[i]` is the **maximum** possible number of nodes that are **target** to node `i` of the first tree if you had to connect one node from the first tree to another node in the second tree.

**Note** that queries are independent from each other. That is, for every query you will remove the added edge before proceeding to the next query.

**Example 1:**

**Input:** edges1 = \[\[0,1],[0,2],[2,3],[2,4]], edges2 = \[\[0,1],[0,2],[0,3],[2,7],[1,4],[4,5],[4,6]]

**Output:** [8,7,7,8,8]

**Explanation:**

*   For `i = 0`, connect node 0 from the first tree to node 0 from the second tree.
*   For `i = 1`, connect node 1 from the first tree to node 4 from the second tree.
*   For `i = 2`, connect node 2 from the first tree to node 7 from the second tree.
*   For `i = 3`, connect node 3 from the first tree to node 0 from the second tree.
*   For `i = 4`, connect node 4 from the first tree to node 4 from the second tree.

![](https://assets.leetcode.com/uploads/2024/09/24/3982-1.png)

**Example 2:**

**Input:** edges1 = \[\[0,1],[0,2],[0,3],[0,4]], edges2 = \[\[0,1],[1,2],[2,3]]

**Output:** [3,6,6,6,6]

**Explanation:**

For every `i`, connect node `i` of the first tree with any node of the second tree.

![](https://assets.leetcode.com/uploads/2024/09/24/3928-2.png)

**Constraints:**

*   <code>2 <= n, m <= 10<sup>5</sup></code>
*   `edges1.length == n - 1`
*   `edges2.length == m - 1`
*   `edges1[i].length == edges2[i].length == 2`
*   <code>edges1[i] = [a<sub>i</sub>, b<sub>i</sub>]</code>
*   <code>0 <= a<sub>i</sub>, b<sub>i</sub> < n</code>
*   <code>edges2[i] = [u<sub>i</sub>, v<sub>i</sub>]</code>
*   <code>0 <= u<sub>i</sub>, v<sub>i</sub> < m</code>
*   The input is generated such that `edges1` and `edges2` represent valid trees.

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun maxTargetNodes(edges1: Array<IntArray>, edges2: Array<IntArray>): IntArray {
        val n = edges1.size + 1
        val g1 = packU(n, edges1)
        val m = edges2.size + 1
        val g2 = packU(m, edges2)
        val p2 = parents(g2)
        val eo2 = IntArray(2)
        for (i in 0..<m) {
            eo2[p2[2][i] % 2]++
        }
        val max = max(eo2[0], eo2[1])
        val p1 = parents(g1)
        val eo1 = IntArray(2)
        for (i in 0..<n) {
            eo1[p1[2][i] % 2]++
        }
        val ans = IntArray(n)
        for (i in 0..<n) {
            ans[i] = eo1[p1[2][i] % 2] + max
        }
        return ans
    }

    private fun parents(g: Array<IntArray>): Array<IntArray> {
        val n = g.size
        val par = IntArray(n)
        par.fill(-1)
        val depth = IntArray(n)
        depth[0] = 0
        val q = IntArray(n)
        q[0] = 0
        var p = 0
        var r = 1
        while (p < r) {
            val cur = q[p]
            for (nex in g[cur]) {
                if (par[cur] != nex) {
                    q[r++] = nex
                    par[nex] = cur
                    depth[nex] = depth[cur] + 1
                }
            }
            p++
        }
        return arrayOf<IntArray>(par, q, depth)
    }

    private fun packU(n: Int, ft: Array<IntArray>): Array<IntArray> {
        val g = Array<IntArray>(n) { IntArray(0) }
        val p = IntArray(n)
        for (u in ft) {
            p[u[0]]++
            p[u[1]]++
        }
        for (i in 0..<n) {
            g[i] = IntArray(p[i])
        }
        for (u in ft) {
            g[u[0]][--p[u[0]]] = u[1]
            g[u[1]][--p[u[1]]] = u[0]
        }
        return g
    }
}
```