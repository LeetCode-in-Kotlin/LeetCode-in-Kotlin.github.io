[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2685\. Count the Number of Complete Components

Medium

You are given an integer `n`. There is an **undirected** graph with `n` vertices, numbered from `0` to `n - 1`. You are given a 2D integer array `edges` where <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> denotes that there exists an **undirected** edge connecting vertices <code>a<sub>i</sub></code> and <code>b<sub>i</sub></code>.

Return _the number of **complete connected components** of the graph_.

A **connected component** is a subgraph of a graph in which there exists a path between any two vertices, and no vertex of the subgraph shares an edge with a vertex outside of the subgraph.

A connected component is said to be **complete** if there exists an edge between every pair of its vertices.

**Example 1:**

**![](https://assets.leetcode.com/uploads/2023/04/11/screenshot-from-2023-04-11-23-31-23.png)**

**Input:** n = 6, edges = \[\[0,1],[0,2],[1,2],[3,4]]

**Output:** 3

**Explanation:** From the picture above, one can see that all of the components of this graph are complete.

**Example 2:**

**![](https://assets.leetcode.com/uploads/2023/04/11/screenshot-from-2023-04-11-23-32-00.png)**

**Input:** n = 6, edges = \[\[0,1],[0,2],[1,2],[3,4],[3,5]]

**Output:** 1

**Explanation:** The component containing vertices 0, 1, and 2 is complete since there is an edge between every pair of two vertices. On the other hand, the component containing vertices 3, 4, and 5 is not complete since there is no edge between vertices 4 and 5. Thus, the number of complete components in this graph is 1.

**Constraints:**

*   `1 <= n <= 50`
*   `0 <= edges.length <= n * (n - 1) / 2`
*   `edges[i].length == 2`
*   <code>0 <= a<sub>i</sub>, b<sub>i</sub> <= n - 1</code>
*   <code>a<sub>i</sub> != b<sub>i</sub></code>
*   There are no repeated edges.

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun countCompleteComponents(n: Int, edges: Array<IntArray>): Int {
        val adj = HashMap<Int, ArrayList<Int>>().apply {
            for ((u, v) in edges) {
                this[u] = getOrDefault(u, arrayListOf()).apply { add(v) }
                this[v] = getOrDefault(v, arrayListOf()).apply { add(u) }
            }
        }
        val visited = BooleanArray(n)
        fun bfs(i: Int): Pair<Int, Int> {
            if (visited[i]) return 0 to 0
            visited[i] = true
            var nodes = 1
            var edges = (adj[i]?.size ?: 0)
            adj[i]?.forEach {
                val (nodes2, edges2) = bfs(it)
                nodes += nodes2
                edges += edges2
            }
            return nodes to edges
        }
        var res = 0
        for (i in 0 until n) {
            if (!visited[i]) {
                val (nodes, edges) = bfs(i)
                if ((nodes * (nodes - 1)) == edges) {
                    res++
                }
            }
        }
        return res
    }
}
```