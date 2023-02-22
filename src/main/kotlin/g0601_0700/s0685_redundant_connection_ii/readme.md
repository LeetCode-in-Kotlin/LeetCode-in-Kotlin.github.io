[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 685\. Redundant Connection II

Hard

In this problem, a rooted tree is a **directed** graph such that, there is exactly one node (the root) for which all other nodes are descendants of this node, plus every node has exactly one parent, except for the root node which has no parents.

The given input is a directed graph that started as a rooted tree with `n` nodes (with distinct values from `1` to `n`), with one additional directed edge added. The added edge has two different vertices chosen from `1` to `n`, and was not an edge that already existed.

The resulting graph is given as a 2D-array of `edges`. Each element of `edges` is a pair <code>[u<sub>i</sub>, v<sub>i</sub>]</code> that represents a **directed** edge connecting nodes <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code>, where <code>u<sub>i</sub></code> is a parent of child <code>v<sub>i</sub></code>.

Return _an edge that can be removed so that the resulting graph is a rooted tree of_ `n` _nodes_. If there are multiple answers, return the answer that occurs last in the given 2D-array.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/12/20/graph1.jpg)

**Input:** edges = \[\[1,2],[1,3],[2,3]]

**Output:** [2,3]

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/12/20/graph2.jpg)

**Input:** edges = \[\[1,2],[2,3],[3,4],[4,1],[1,5]]

**Output:** [4,1]

**Constraints:**

*   `n == edges.length`
*   `3 <= n <= 1000`
*   `edges[i].length == 2`
*   <code>1 <= u<sub>i</sub>, v<sub>i</sub> <= n</code>
*   <code>u<sub>i</sub> != v<sub>i</sub></code>

## Solution

```kotlin
class Solution {
    private lateinit var par: IntArray
    fun findRedundantDirectedConnection(edges: Array<IntArray>): IntArray {
        val n = edges.size
        val hasPar = IntArray(n + 1)
        for (edge in edges) {
            val v = edge[1]
            hasPar[v]++
        }
        par = IntArray(n + 1)
        for (i in par.indices) {
            par[i] = i
        }
        for (edge in edges) {
            val u = edge[0]
            val v = edge[1]
            if (hasPar[v] == 1) {
                val lu = find(u)
                val lv = find(v)
                if (lu != lv) {
                    par[lu] = lv
                } else {
                    return edge
                }
            }
        }
        for (edge in edges) {
            val u = edge[0]
            val v = edge[1]
            if (hasPar[v] > 1) {
                val lu = find(u)
                val lv = find(v)
                if (lu != lv) {
                    par[lu] = lv
                } else {
                    return edge
                }
            }
        }
        return IntArray(2)
    }

    private fun find(x: Int): Int {
        return if (par[x] == x) {
            x
        } else find(par[x])
    }
}
```