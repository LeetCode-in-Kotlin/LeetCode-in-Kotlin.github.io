[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2846\. Minimum Edge Weight Equilibrium Queries in a Tree

Hard

There is an undirected tree with `n` nodes labeled from `0` to `n - 1`. You are given the integer `n` and a 2D integer array `edges` of length `n - 1`, where <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code> indicates that there is an edge between nodes <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code> with weight <code>w<sub>i</sub></code> in the tree.

You are also given a 2D integer array `queries` of length `m`, where <code>queries[i] = [a<sub>i</sub>, b<sub>i</sub>]</code>. For each query, find the **minimum number of operations** required to make the weight of every edge on the path from <code>a<sub>i</sub></code> to <code>b<sub>i</sub></code> equal. In one operation, you can choose any edge of the tree and change its weight to any value.

**Note** that:

*   Queries are **independent** of each other, meaning that the tree returns to its **initial state** on each new query.
*   The path from <code>a<sub>i</sub></code> to <code>b<sub>i</sub></code> is a sequence of **distinct** nodes starting with node <code>a<sub>i</sub></code> and ending with node <code>b<sub>i</sub></code> such that every two adjacent nodes in the sequence share an edge in the tree.

Return _an array_ `answer` _of length_ `m` _where_ `answer[i]` _is the answer to the_ <code>i<sup>th</sup></code> _query._

**Example 1:**

![](https://assets.leetcode.com/uploads/2023/08/11/graph-6-1.png)

**Input:** n = 7, edges = \[\[0,1,1],[1,2,1],[2,3,1],[3,4,2],[4,5,2],[5,6,2]], queries = \[\[0,3],[3,6],[2,6],[0,6]]

**Output:** [0,0,1,3]

**Explanation:** In the first query, all the edges in the path from 0 to 3 have a weight of 1. Hence, the answer is 0.

In the second query, all the edges in the path from 3 to 6 have a weight of 2. Hence, the answer is 0. In the third query, we change the weight of edge [2,3] to 2. After this operation, all the edges in the path from 2 to 6 have a weight of 2. Hence, the answer is 1.

In the fourth query, we change the weights of edges [0,1], [1,2] and [2,3] to 2. After these operations, all the edges in the path from 0 to 6 have a weight of 2. Hence, the answer is 3. For each queries[i], it can be shown that answer[i] is the minimum number of operations needed to equalize all the edge weights in the path from a<sub>i</sub> to b<sub>i</sub>.

**Example 2:**

![](https://assets.leetcode.com/uploads/2023/08/11/graph-9-1.png)

**Input:** n = 8, edges = \[\[1,2,6],[1,3,4],[2,4,6],[2,5,3],[3,6,6],[3,0,8],[7,0,2]], queries = \[\[4,6],[0,4],[6,5],[7,4]]

**Output:** [1,2,2,3]

**Explanation:** In the first query, we change the weight of edge [1,3] to 6. After this operation, all the edges in the path from 4 to 6 have a weight of 6. Hence, the answer is 1.

In the second query, we change the weight of edges [0,3] and [3,1] to 6. After these operations, all the edges in the path from 0 to 4 have a weight of 6. Hence, the answer is 2.

In the third query, we change the weight of edges [1,3] and [5,2] to 6. After these operations, all the edges in the path from 6 to 5 have a weight of 6. Hence, the answer is 2.

In the fourth query, we change the weights of edges [0,7], [0,3] and [1,3] to 6. After these operations, all the edges in the path from 7 to 4 have a weight of 6. Hence, the answer is 3. For each queries[i], it can be shown that answer[i] is the minimum number of operations needed to equalize all the edge weights in the path from a<sub>i</sub> to b<sub>i</sub>.

**Constraints:**

*   <code>1 <= n <= 10<sup>4</sup></code>
*   `edges.length == n - 1`
*   `edges[i].length == 3`
*   <code>0 <= u<sub>i</sub>, v<sub>i</sub> < n</code>
*   <code>1 <= w<sub>i</sub> <= 26</code>
*   The input is generated such that `edges` represents a valid tree.
*   <code>1 <= queries.length == m <= 2 * 10<sup>4</sup></code>
*   `queries[i].length == 2`
*   <code>0 <= a<sub>i</sub>, b<sub>i</sub> < n</code>

## Solution

```kotlin
import kotlin.math.ln
import kotlin.math.max

@Suppress("kotlin:S107")
class Solution {
    private class Node(var v: Int, var w: Int)

    fun minOperationsQueries(n: Int, edges: Array<IntArray>, queries: Array<IntArray>): IntArray {
        val graph = createGraph(edges, n)
        val queryCount = queries.size
        val res = IntArray(queryCount)
        val parent = IntArray(n)
        val level = IntArray(n)
        val weightFreq = Array(n) { IntArray(27) }
        val freq = IntArray(27)
        val height = (ln(n.toDouble()) / ln(2.0)).toInt() + 1
        val up = Array(n) { IntArray(height) }
        for (arr in up) {
            arr.fill(-1)
        }
        dfs(graph, 0, 0, -1, parent, level, weightFreq, freq)
        for (i in 0 until n) {
            up[i][0] = parent[i]
        }
        for (i in 1 until height) {
            for (j in 0 until n) {
                if (up[j][i - 1] == -1) {
                    up[j][i] = -1
                    continue
                }
                up[j][i] = up[up[j][i - 1]][i - 1]
            }
        }
        for (i in 0 until queryCount) {
            val src = queries[i][0]
            val dest = queries[i][1]
            val lcaNode = lca(src, dest, up, height, level)
            res[i] = processResult(weightFreq[src], weightFreq[dest], weightFreq[lcaNode])
        }
        return res
    }

    private fun lca(src: Int, dest: Int, up: Array<IntArray>, height: Int, level: IntArray): Int {
        var curr1 = src
        var curr2 = dest
        val minlevel: Int
        if (level[curr1] > level[curr2]) {
            minlevel = level[curr2]
            curr1 = getKthAncestor(curr1, level[curr1] - level[curr2], up, height)
        } else if (level[curr1] <= level[curr2]) {
            minlevel = level[curr1]
            curr2 = getKthAncestor(curr2, level[curr2] - level[curr1], up, height)
        } else {
            minlevel = level[curr1]
        }
        if (curr1 == curr2) {
            return curr1
        }
        var l = 0
        var h = level[curr2]
        while (l <= h) {
            val mid = l + (h - l) / 2
            val p1 = getKthAncestor(curr1, minlevel - mid, up, height)
            val p2 = getKthAncestor(curr2, minlevel - mid, up, height)
            if (p1 == p2) {
                l = mid + 1
            } else {
                h = mid - 1
            }
        }
        return getKthAncestor(curr1, minlevel - l + 1, up, height)
    }

    private fun getKthAncestor(node: Int, k: Int, up: Array<IntArray>, height: Int): Int {
        var curr = node
        var i = 0
        while (i < height && k shr i != 0) {
            if (((1 shl i) and k) != 0) {
                if (curr == -1) {
                    return -1
                }
                curr = up[curr][i]
            }
            i++
        }
        return curr
    }

    private fun processResult(freqSrc: IntArray, freqDest: IntArray, freqLCA: IntArray): Int {
        val freqPath = IntArray(27)
        for (i in 1..26) {
            freqPath[i] = freqSrc[i] + freqDest[i] - 2 * freqLCA[i]
        }
        var max = 0
        var pathlen = 0
        for (i in 1..26) {
            max = max(max.toDouble(), freqPath[i].toDouble()).toInt()
            pathlen += freqPath[i]
        }
        return pathlen - max
    }

    private fun dfs(
        graph: List<MutableList<Node>>,
        src: Int,
        currlevel: Int,
        p: Int,
        parent: IntArray,
        level: IntArray,
        weightFreq: Array<IntArray>,
        freq: IntArray,
    ) {
        parent[src] = p
        level[src] = currlevel
        System.arraycopy(freq, 0, weightFreq[src], 0, freq.size)
        for (node in graph[src]) {
            val v = node.v
            val w = node.w
            if (v != p) {
                freq[w]++
                dfs(graph, v, currlevel + 1, src, parent, level, weightFreq, freq)
                freq[w]--
            }
        }
    }

    private fun createGraph(edges: Array<IntArray>, n: Int): List<MutableList<Node>> {
        val graph: MutableList<MutableList<Node>> = ArrayList()
        for (i in 0 until n) {
            graph.add(ArrayList())
        }
        for (edge in edges) {
            val u = edge[0]
            val v = edge[1]
            val w = edge[2]
            graph[u].add(Node(v, w))
            graph[v].add(Node(u, w))
        }
        return graph
    }
}
```