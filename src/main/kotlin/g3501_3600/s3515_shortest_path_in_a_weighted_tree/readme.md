[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3515\. Shortest Path in a Weighted Tree

Hard

You are given an integer `n` and an undirected, weighted tree rooted at node 1 with `n` nodes numbered from 1 to `n`. This is represented by a 2D array `edges` of length `n - 1`, where <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code> indicates an undirected edge from node <code>u<sub>i</sub></code> to <code>v<sub>i</sub></code> with weight <code>w<sub>i</sub></code>.

You are also given a 2D integer array `queries` of length `q`, where each `queries[i]` is either:

*   `[1, u, v, w']` – **Update** the weight of the edge between nodes `u` and `v` to `w'`, where `(u, v)` is guaranteed to be an edge present in `edges`.
*   `[2, x]` – **Compute** the **shortest** path distance from the root node 1 to node `x`.

Return an integer array `answer`, where `answer[i]` is the **shortest** path distance from node 1 to `x` for the <code>i<sup>th</sup></code> query of `[2, x]`.

**Example 1:**

**Input:** n = 2, edges = \[\[1,2,7]], queries = \[\[2,2],[1,1,2,4],[2,2]]

**Output:** [7,4]

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/03/13/screenshot-2025-03-13-at-133524.png)

*   Query `[2,2]`: The shortest path from root node 1 to node 2 is 7.
*   Query `[1,1,2,4]`: The weight of edge `(1,2)` changes from 7 to 4.
*   Query `[2,2]`: The shortest path from root node 1 to node 2 is 4.

**Example 2:**

**Input:** n = 3, edges = \[\[1,2,2],[1,3,4]], queries = \[\[2,1],[2,3],[1,1,3,7],[2,2],[2,3]]

**Output:** [0,4,2,7]

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/03/13/screenshot-2025-03-13-at-132247.png)

*   Query `[2,1]`: The shortest path from root node 1 to node 1 is 0.
*   Query `[2,3]`: The shortest path from root node 1 to node 3 is 4.
*   Query `[1,1,3,7]`: The weight of edge `(1,3)` changes from 4 to 7.
*   Query `[2,2]`: The shortest path from root node 1 to node 2 is 2.
*   Query `[2,3]`: The shortest path from root node 1 to node 3 is 7.

**Example 3:**

**Input:** n = 4, edges = \[\[1,2,2],[2,3,1],[3,4,5]], queries = \[\[2,4],[2,3],[1,2,3,3],[2,2],[2,3]]

**Output:** [8,3,2,5]

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/03/13/screenshot-2025-03-13-at-133306.png)

*   Query `[2,4]`: The shortest path from root node 1 to node 4 consists of edges `(1,2)`, `(2,3)`, and `(3,4)` with weights `2 + 1 + 5 = 8`.
*   Query `[2,3]`: The shortest path from root node 1 to node 3 consists of edges `(1,2)` and `(2,3)` with weights `2 + 1 = 3`.
*   Query `[1,2,3,3]`: The weight of edge `(2,3)` changes from 1 to 3.
*   Query `[2,2]`: The shortest path from root node 1 to node 2 is 2.
*   Query `[2,3]`: The shortest path from root node 1 to node 3 consists of edges `(1,2)` and `(2,3)` with updated weights `2 + 3 = 5`.

**Constraints:**

*   <code>1 <= n <= 10<sup>5</sup></code>
*   `edges.length == n - 1`
*   <code>edges[i] == [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code>
*   <code>1 <= u<sub>i</sub>, v<sub>i</sub> <= n</code>
*   <code>1 <= w<sub>i</sub> <= 10<sup>4</sup></code>
*   The input is generated such that `edges` represents a valid tree.
*   <code>1 <= queries.length == q <= 10<sup>5</sup></code>
*   `queries[i].length == 2` or `4`
    *   `queries[i] == [1, u, v, w']` or,
    *   `queries[i] == [2, x]`
    *   `1 <= u, v, x <= n`
    *   `(u, v)` is always an edge from `edges`.
    *   <code>1 <= w' <= 10<sup>4</sup></code>

## Solution

```kotlin
class Solution {
    private lateinit var `in`: IntArray
    private lateinit var out: IntArray
    private lateinit var baseDist: IntArray
    private lateinit var parent: IntArray
    private lateinit var depth: IntArray
    private var timer = 0
    private lateinit var edgeWeight: IntArray
    private lateinit var adj: Array<MutableList<IntArray>>

    fun treeQueries(n: Int, edges: Array<IntArray>, queries: Array<IntArray>): IntArray {
        adj = Array<MutableList<IntArray>>(n + 1) { ArrayList<IntArray>() }
        for (e in edges) {
            val u = e[0]
            val v = e[1]
            val w = e[2]
            adj[u].add(intArrayOf(v, w))
            adj[v].add(intArrayOf(u, w))
        }
        `in` = IntArray(n + 1)
        out = IntArray(n + 1)
        baseDist = IntArray(n + 1)
        parent = IntArray(n + 1)
        depth = IntArray(n + 1)
        edgeWeight = IntArray(n + 1)
        dfs(1, 0, 0)
        val fenw = Fen(n)
        val ansList: MutableList<Int> = ArrayList<Int>()
        for (query in queries) {
            if (query[0] == 1) {
                val u = query[1]
                val v = query[2]
                val newW = query[3]
                val child: Int
                if (parent[v] == u) {
                    child = v
                } else if (parent[u] == v) {
                    child = u
                } else {
                    continue
                }
                val diff = newW - edgeWeight[child]
                edgeWeight[child] = newW
                fenw.updateRange(`in`[child], out[child], diff)
            } else {
                val x = query[1]
                val delta = fenw.query(`in`[x])
                ansList.add(baseDist[x] + delta)
            }
        }
        val answer = IntArray(ansList.size)
        for (i in ansList.indices) {
            answer[i] = ansList[i]
        }
        return answer
    }

    private fun dfs(node: Int, par: Int, dist: Int) {
        parent[node] = par
        baseDist[node] = dist
        depth[node] = if (par == 0) 0 else depth[par] + 1
        `in`[node] = ++timer
        for (neighborInfo in adj[node]) {
            val neighbor = neighborInfo[0]
            val w = neighborInfo[1]
            if (neighbor == par) {
                continue
            }
            edgeWeight[neighbor] = w
            dfs(neighbor, node, dist + w)
        }
        out[node] = timer
    }

    private class Fen(var n: Int) {
        var fenw: IntArray = IntArray(n + 2)

        fun update(i: Int, delta: Int) {
            var i = i
            while (i <= n) {
                fenw[i] += delta
                i += i and -i
            }
        }

        fun updateRange(l: Int, r: Int, delta: Int) {
            update(l, delta)
            update(r + 1, -delta)
        }

        fun query(i: Int): Int {
            var i = i
            var sum = 0
            while (i > 0) {
                sum += fenw[i]
                i -= i and -i
            }
            return sum
        }
    }
}
```