[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2192\. All Ancestors of a Node in a Directed Acyclic Graph

Medium

You are given a positive integer `n` representing the number of nodes of a **Directed Acyclic Graph** (DAG). The nodes are numbered from `0` to `n - 1` (**inclusive**).

You are also given a 2D integer array `edges`, where <code>edges[i] = [from<sub>i</sub>, to<sub>i</sub>]</code> denotes that there is a **unidirectional** edge from <code>from<sub>i</sub></code> to <code>to<sub>i</sub></code> in the graph.

Return _a list_ `answer`_, where_ `answer[i]` _is the **list of ancestors** of the_ <code>i<sup>th</sup></code> _node, sorted in **ascending order**_.

A node `u` is an **ancestor** of another node `v` if `u` can reach `v` via a set of edges.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/12/12/e1.png)

**Input:** n = 8, edgeList = \[\[0,3],[0,4],[1,3],[2,4],[2,7],[3,5],[3,6],[3,7],[4,6]]

**Output:** [[],[],[],[0,1],[0,2],[0,1,3],[0,1,2,3,4],[0,1,2,3]]

**Explanation:**

The above diagram represents the input graph.

- Nodes 0, 1, and 2 do not have any ancestors.

- Node 3 has two ancestors 0 and 1.

- Node 4 has two ancestors 0 and 2.

- Node 5 has three ancestors 0, 1, and 3.

- Node 6 has five ancestors 0, 1, 2, 3, and 4.

- Node 7 has four ancestors 0, 1, 2, and 3. 

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/12/12/e2.png)

**Input:** n = 5, edgeList = \[\[0,1],[0,2],[0,3],[0,4],[1,2],[1,3],[1,4],[2,3],[2,4],[3,4]]

**Output:** [[],[0],[0,1],[0,1,2],[0,1,2,3]]

**Explanation:**

The above diagram represents the input graph.

- Node 0 does not have any ancestor.

- Node 1 has one ancestor 0.

- Node 2 has two ancestors 0 and 1.

- Node 3 has three ancestors 0, 1, and 2.

- Node 4 has four ancestors 0, 1, 2, and 3. 

**Constraints:**

*   `1 <= n <= 1000`
*   `0 <= edges.length <= min(2000, n * (n - 1) / 2)`
*   `edges[i].length == 2`
*   <code>0 <= from<sub>i</sub>, to<sub>i</sub> <= n - 1</code>
*   <code>from<sub>i</sub> != to<sub>i</sub></code>
*   There are no duplicate edges.
*   The graph is **directed** and **acyclic**.

## Solution

```kotlin
class Solution {
    private lateinit var adjList: MutableList<MutableList<Int>>
    private lateinit var result: MutableList<MutableList<Int>>

    fun getAncestors(n: Int, edges: Array<IntArray>): List<MutableList<Int>> {
        adjList = ArrayList()
        result = ArrayList()
        for (i in 0 until n) {
            adjList.add(ArrayList())
            result.add(ArrayList())
        }
        for (edge in edges) {
            val start = edge[0]
            val end = edge[1]
            adjList[start].add(end)
        }
        //  DFS for each node from 0 --> n , and add that node as root/parent into each reachable
        // node and their child
        //  Use visited[] to identify if any of the child or their childs are already visited for
        // that perticular root/parent,
        //  so will not add the root to avoid duplicacy and call reduction .
        for (i in 0 until n) {
            val visited = BooleanArray(n)
            val childList: List<Int> = adjList[i]
            for (child in childList) {
                if (!visited[child]) {
                    dfs(i, child, visited)
                }
            }
        }
        return result
    }

    private fun dfs(root: Int, node: Int, visited: BooleanArray) {
        if (visited[node]) {
            return
        }
        visited[node] = true
        result[node].add(root)
        val childList: List<Int> = adjList[node]
        for (child in childList) {
            if (!visited[child]) {
                dfs(root, child, visited)
            }
        }
    }
}
```