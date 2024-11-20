[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2699\. Modify Graph Edge Weights

Hard

You are given an **undirected weighted** **connected** graph containing `n` nodes labeled from `0` to `n - 1`, and an integer array `edges` where <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>, w<sub>i</sub>]</code> indicates that there is an edge between nodes <code>a<sub>i</sub></code> and <code>b<sub>i</sub></code> with weight <code>w<sub>i</sub></code>.

Some edges have a weight of `-1` (<code>w<sub>i</sub> = -1</code>), while others have a **positive** weight (<code>w<sub>i</sub> > 0</code>).

Your task is to modify **all edges** with a weight of `-1` by assigning them **positive integer values** in the range <code>[1, 2 * 10<sup>9</sup>]</code> so that the **shortest distance** between the nodes `source` and `destination` becomes equal to an integer `target`. If there are **multiple** **modifications** that make the shortest distance between `source` and `destination` equal to `target`, any of them will be considered correct.

Return _an array containing all edges (even unmodified ones) in any order if it is possible to make the shortest distance from_ `source` _to_ `destination` _equal to_ `target`_, or an **empty array** if it's impossible._

**Note:** You are not allowed to modify the weights of edges with initial positive weights.

**Example 1:**

**![](https://assets.leetcode.com/uploads/2023/04/18/graph.png)**

**Input:** n = 5, edges = \[\[4,1,-1],[2,0,-1],[0,3,-1],[4,3,-1]], source = 0, destination = 1, target = 5

**Output:** [[4,1,1],[2,0,1],[0,3,3],[4,3,1]]

**Explanation:** The graph above shows a possible modification to the edges, making the distance from 0 to 1 equal to 5.

**Example 2:**

**![](https://assets.leetcode.com/uploads/2023/04/18/graph-2.png)**

**Input:** n = 3, edges = \[\[0,1,-1],[0,2,5]], source = 0, destination = 2, target = 6

**Output:** []

**Explanation:** The graph above contains the initial edges. It is not possible to make the distance from 0 to 2 equal to 6 by modifying the edge with weight -1. So, an empty array is returned.

**Example 3:**

**![](https://assets.leetcode.com/uploads/2023/04/19/graph-3.png)**

**Input:** n = 4, edges = \[\[1,0,4],[1,2,3],[2,3,5],[0,3,-1]], source = 0, destination = 2, target = 6

**Output:** [[1,0,4],[1,2,3],[2,3,5],[0,3,1]]

**Explanation:** The graph above shows a modified graph having the shortest distance from 0 to 2 as 6.

**Constraints:**

*   `1 <= n <= 100`
*   `1 <= edges.length <= n * (n - 1) / 2`
*   `edges[i].length == 3`
*   <code>0 <= a<sub>i</sub>, b<sub>i </sub>< n</code>
*   <code>w<sub>i</sub> = -1 </code>or <code>1 <= w<sub>i </sub><= 10<sup>7</sup></code>
*   <code>a<sub>i </sub>!= b<sub>i</sub></code>
*   `0 <= source, destination < n`
*   `source != destination`
*   <code>1 <= target <= 10<sup>9</sup></code>
*   The graph is connected, and there are no self-loops or repeated edges

## Solution

```kotlin
import java.util.PriorityQueue

class Solution {
    private inner class Edge(var s: Int, var d: Int, var wt: Int)
    private lateinit var g: Array<ArrayList<Edge>?>
    private var n = 0
    private var t = 0
    fun modifiedGraphEdges(
        n: Int,
        edges: Array<IntArray>,
        source: Int,
        destination: Int,
        target: Int,
    ): Array<IntArray> {
        this.n = n
        g = arrayOfNulls(n)
        t = target
        for (i in 0 until n) {
            g[i] = ArrayList()
        }
        for (e in edges) {
            val s = e[0]
            val d = e[1]
            val wt = e[2]
            if (wt == -1) continue
            g[s]!!.add(Edge(s, d, wt))
            g[d]!!.add(Edge(d, s, wt))
        }
        val inc = shortestPath(source, destination)
        if (inc != -1 && inc < target) return Array(0) { IntArray(0) }
        if (inc == target) {
            ntomax(edges)
            return edges
        }
        for (e in edges) {
            if (e[2] == -1) {
                e[2] = 1
                val s = e[0]
                val d = e[1]
                val wt = e[2]
                g[s]!!.add(Edge(s, d, wt))
                g[d]!!.add(Edge(d, s, wt))
                val cost = shortestPath(source, destination)
                if (cost == -1) continue
                if (cost < target) {
                    e[2] = target - cost + 1
                    ntomax(edges)
                    return edges
                }
                if (cost == target) {
                    ntomax(edges)
                    return edges
                }
            }
        }
        return Array(0) { IntArray(0) }
    }

    private fun ntomax(edges: Array<IntArray>) {
        for (e in edges) {
            if (e[2] == -1) {
                e[2] = 1000000000
            }
        }
    }

    private inner class Pair(var s: Int, var cost: Int) : Comparable<Pair?> {
        override operator fun compareTo(other: Pair?): Int {
            return cost - other!!.cost
        }
    }

    private fun shortestPath(s: Int, d: Int): Int {
        val q = PriorityQueue<Pair>()
        q.add(Pair(s, 0))
        val vis = BooleanArray(n)
        while (q.isNotEmpty()) {
            val rem = q.remove()
            vis[rem.s] = true
            if (rem.s == d) return rem.cost
            for (e in g[rem.s]!!) {
                if (!vis[e.d]) {
                    q.add(Pair(e.d, rem.cost + e.wt))
                }
            }
        }
        return -1
    }
}
```