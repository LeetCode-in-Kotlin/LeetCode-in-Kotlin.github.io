[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1129\. Shortest Path with Alternating Colors

Medium

You are given an integer `n`, the number of nodes in a directed graph where the nodes are labeled from `0` to `n - 1`. Each edge is red or blue in this graph, and there could be self-edges and parallel edges.

You are given two arrays `redEdges` and `blueEdges` where:

*   <code>redEdges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> indicates that there is a directed red edge from node <code>a<sub>i</sub></code> to node <code>b<sub>i</sub></code> in the graph, and
*   <code>blueEdges[j] = [u<sub>j</sub>, v<sub>j</sub>]</code> indicates that there is a directed blue edge from node <code>u<sub>j</sub></code> to node <code>v<sub>j</sub></code> in the graph.

Return an array `answer` of length `n`, where each `answer[x]` is the length of the shortest path from node `0` to node `x` such that the edge colors alternate along the path, or `-1` if such a path does not exist.

**Example 1:**

**Input:** n = 3, redEdges = \[\[0,1],[1,2]], blueEdges = []

**Output:** [0,1,-1]

**Example 2:**

**Input:** n = 3, redEdges = \[\[0,1]], blueEdges = \[\[2,1]]

**Output:** [0,1,-1]

**Constraints:**

*   `1 <= n <= 100`
*   `0 <= redEdges.length, blueEdges.length <= 400`
*   `redEdges[i].length == blueEdges[j].length == 2`
*   <code>0 <= a<sub>i</sub>, b<sub>i</sub>, u<sub>j</sub>, v<sub>j</sub> < n</code>

## Solution

```kotlin
import java.util.LinkedList
import java.util.Queue

@Suppress("NAME_SHADOWING")
class Solution {
    private class Pair(var node: Int, var color: Int)

    private fun bfs(
        q: Queue<Int>,
        vis: Array<BooleanArray>,
        graph: List<MutableList<Pair>>,
        blue: Boolean,
        shortestPaths: IntArray,
    ) {
        var blue = blue
        var level = 0
        q.add(0)
        if (blue) {
            vis[0][1] = true
        } else {
            vis[0][0] = true
        }
        while (q.isNotEmpty()) {
            var size = q.size
            while (size-- > 0) {
                val curr = q.poll()
                shortestPaths[curr] = Math.min(level, shortestPaths[curr])
                for (nextNode in graph[curr]) {
                    if (nextNode.color == 1 && blue && !vis[nextNode.node][1]) {
                        q.add(nextNode.node)
                        vis[nextNode.node][1] = true
                    }
                    if (!blue && nextNode.color == 0 && !vis[nextNode.node][0]) {
                        q.add(nextNode.node)
                        vis[nextNode.node][0] = true
                    }
                }
            }
            blue = !blue
            level++
        }
    }

    fun shortestAlternatingPaths(n: Int, redEdges: Array<IntArray>, blueEdges: Array<IntArray>): IntArray {
        val graph: MutableList<MutableList<Pair>> = ArrayList()
        for (i in 0 until n) {
            graph.add(mutableListOf())
        }
        for (edge in redEdges) {
            val a = edge[0]
            val b = edge[1]
            // red -> 0
            graph[a].add(Pair(b, 0))
        }
        for (edge in blueEdges) {
            val u = edge[0]
            val v = edge[1]
            // blue -> 1
            graph[u].add(Pair(v, 1))
        }
        val shortestPaths = IntArray(n)
        val q: Queue<Int> = LinkedList()
        shortestPaths.fill(Int.MAX_VALUE)
        bfs(q, Array(n) { BooleanArray(2) }, graph, true, shortestPaths)
        bfs(q, Array(n) { BooleanArray(2) }, graph, false, shortestPaths)
        for (i in 0 until n) {
            if (shortestPaths[i] == Int.MAX_VALUE) {
                shortestPaths[i] = -1
            }
        }
        return shortestPaths
    }
}
```