[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1443\. Minimum Time to Collect All Apples in a Tree

Medium

Given an undirected tree consisting of `n` vertices numbered from `0` to `n-1`, which has some apples in their vertices. You spend 1 second to walk over one edge of the tree. _Return the minimum time in seconds you have to spend to collect all apples in the tree, starting at **vertex 0** and coming back to this vertex._

The edges of the undirected tree are given in the array `edges`, where <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> means that exists an edge connecting the vertices <code>a<sub>i</sub></code> and <code>b<sub>i</sub></code>. Additionally, there is a boolean array `hasApple`, where `hasApple[i] = true` means that vertex `i` has an apple; otherwise, it does not have any apple.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/04/23/min_time_collect_apple_1.png)

**Input:** n = 7, edges = \[\[0,1],[0,2],[1,4],[1,5],[2,3],[2,6]], hasApple = [false,false,true,false,true,true,false]

**Output:** 8

**Explanation:** The figure above represents the given tree where red vertices have an apple. One optimal path to collect all apples is shown by the green arrows.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/04/23/min_time_collect_apple_2.png)

**Input:** n = 7, edges = \[\[0,1],[0,2],[1,4],[1,5],[2,3],[2,6]], hasApple = [false,false,true,false,false,true,false]

**Output:** 6

**Explanation:** The figure above represents the given tree where red vertices have an apple. One optimal path to collect all apples is shown by the green arrows.

**Example 3:**

**Input:** n = 7, edges = \[\[0,1],[0,2],[1,4],[1,5],[2,3],[2,6]], hasApple = [false,false,false,false,false,false,false]

**Output:** 0

**Constraints:**

*   <code>1 <= n <= 10<sup>5</sup></code>
*   `edges.length == n - 1`
*   `edges[i].length == 2`
*   <code>0 <= a<sub>i</sub> < b<sub>i</sub> <= n - 1</code>
*   <code>from<sub>i</sub> < to<sub>i</sub></code>
*   `hasApple.length == n`

## Solution

```kotlin
@Suppress("UNUSED_PARAMETER")
class Solution {
    fun minTime(n: Int, edges: Array<IntArray>, hasApple: List<Boolean>): Int {
        val visited: MutableSet<Int> = HashSet()
        val graph: MutableMap<Int, MutableList<Int>> = HashMap()
        for (edge in edges) {
            val vertexA = edge[0]
            val vertexB = edge[1]
            graph.computeIfAbsent(vertexA) { _: Int? -> ArrayList() }.add(vertexB)
            graph.computeIfAbsent(vertexB) { _: Int? -> ArrayList() }.add(vertexA)
        }
        visited.add(0)
        val steps = helper(graph, hasApple, 0, visited)
        return if (steps > 0) steps - 2 else 0
    }

    private fun helper(
        graph: Map<Int, MutableList<Int>>,
        hasApple: List<Boolean>,
        node: Int,
        visited: MutableSet<Int>
    ): Int {
        var steps = 0
        for (child in graph.getOrDefault(node, mutableListOf())) {
            if (visited.contains(child)) {
                continue
            } else {
                visited.add(child)
            }
            steps += helper(graph, hasApple, child, visited)
        }
        return if (steps > 0) {
            steps + 2
        } else if (java.lang.Boolean.TRUE == hasApple[node]) {
            2
        } else {
            0
        }
    }
}
```