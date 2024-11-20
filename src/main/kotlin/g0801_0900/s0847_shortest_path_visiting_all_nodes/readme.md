[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 847\. Shortest Path Visiting All Nodes

Hard

You have an undirected, connected graph of `n` nodes labeled from `0` to `n - 1`. You are given an array `graph` where `graph[i]` is a list of all the nodes connected with node `i` by an edge.

Return _the length of the shortest path that visits every node_. You may start and stop at any node, you may revisit nodes multiple times, and you may reuse edges.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/05/12/shortest1-graph.jpg)

**Input:** graph = \[\[1,2,3],[0],[0],[0]]

**Output:** 4

**Explanation:** One possible path is [1,0,2,0,3]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/05/12/shortest2-graph.jpg)

**Input:** graph = \[\[1],[0,2,4],[1,3,4],[2],[1,2]]

**Output:** 4

**Explanation:** One possible path is [0,1,4,2,3]

**Constraints:**

*   `n == graph.length`
*   `1 <= n <= 12`
*   `0 <= graph[i].length < n`
*   `graph[i]` does not contain `i`.
*   If `graph[a]` contains `b`, then `graph[b]` contains `a`.
*   The input graph is always connected.

## Solution

```kotlin
import java.util.LinkedList
import java.util.Objects
import java.util.Queue

class Solution {
    fun shortestPathLength(graph: Array<IntArray>): Int {
        val target = (1 shl graph.size) - 1
        val q: Queue<IntArray> = LinkedList()
        for (i in graph.indices) {
            q.offer(intArrayOf(i, 1 shl i))
        }
        var steps = 0
        val visited = Array(graph.size) {
            BooleanArray(
                target + 1,
            )
        }
        while (q.isNotEmpty()) {
            val size = q.size
            for (i in 0 until size) {
                val curr = q.poll()
                val currNode = Objects.requireNonNull(curr)[0]
                val currState = curr[1]
                if (currState == target) {
                    return steps
                }
                for (n in graph[currNode]) {
                    val newState = currState or (1 shl n)
                    if (visited[n][newState]) {
                        continue
                    }
                    visited[n][newState] = true
                    q.offer(intArrayOf(n, newState))
                }
            }
            ++steps
        }
        return -1
    }
}
```