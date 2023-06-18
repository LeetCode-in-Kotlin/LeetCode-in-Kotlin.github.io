[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1617\. Count Subtrees With Max Distance Between Cities

Hard

There are `n` cities numbered from `1` to `n`. You are given an array `edges` of size `n-1`, where <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> represents a bidirectional edge between cities <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code>. There exists a unique path between each pair of cities. In other words, the cities form a **tree**.

A **subtree** is a subset of cities where every city is reachable from every other city in the subset, where the path between each pair passes through only the cities from the subset. Two subtrees are different if there is a city in one subtree that is not present in the other.

For each `d` from `1` to `n-1`, find the number of subtrees in which the **maximum distance** between any two cities in the subtree is equal to `d`.

Return _an array of size_ `n-1` _where the_ <code>d<sup>th</sup></code> _element **(1-indexed)** is the number of subtrees in which the **maximum distance** between any two cities is equal to_ `d`.

**Notice** that the **distance** between the two cities is the number of edges in the path between them.

**Example 1:**

**![](https://assets.leetcode.com/uploads/2020/09/21/p1.png)**

**Input:** n = 4, edges = \[\[1,2],[2,3],[2,4]]

**Output:** [3,4,0]

**Explanation:** 

The subtrees with subsets {1,2}, {2,3} and {2,4} have a max distance of 1. 

The subtrees with subsets {1,2,3}, {1,2,4}, {2,3,4} and {1,2,3,4} have a max distance of 2. 

No subtree has two nodes where the max distance between them is 3.

**Example 2:**

**Input:** n = 2, edges = \[\[1,2]]

**Output:** [1]

**Example 3:**

**Input:** n = 3, edges = \[\[1,2],[2,3]]

**Output:** [2,1]

**Constraints:**

*   `2 <= n <= 15`
*   `edges.length == n-1`
*   `edges[i].length == 2`
*   <code>1 <= u<sub>i</sub>, v<sub>i</sub> <= n</code>
*   All pairs <code>(u<sub>i</sub>, v<sub>i</sub>)</code> are distinct.

## Solution

```kotlin
import kotlin.math.pow

class Solution {
    private var ans = 0
    private var vis = 0

    fun countSubgraphsForEachDiameter(n: Int, edges: Array<IntArray>): IntArray {
        ans = 0
        vis = 0
        val dist = IntArray(n - 1)
        val graph: MutableMap<Int, MutableList<Int>> = HashMap()
        for (i in edges) {
            graph.computeIfAbsent(1 shl i[0] - 1) { initialCapacity: Int? ->
                ArrayList(
                    initialCapacity!!
                )
            }.add(1 shl i[1] - 1)
            graph.computeIfAbsent(1 shl i[1] - 1) { initialCapacity: Int? ->
                ArrayList(
                    initialCapacity!!
                )
            }.add(1 shl i[0] - 1)
        }
        val ps = 2.0.pow(n.toDouble()).toInt() - 1
        for (set in 3..ps) {
            // is power of 2
            val isp2 = set != 0 && set and set - 1 == 0
            if (!isp2) {
                ans = 0
                vis = 0
                dfs(graph, set, Integer.highestOneBit(set), -1)
                if (vis == set) {
                    dist[ans - 1]++
                }
            }
        }
        return dist
    }

    private fun dfs(graph: Map<Int, MutableList<Int>>, set: Int, c: Int, p: Int): Int {
        if (set and c == 0) {
            return 0
        }
        vis = vis or c
        var fdist = 0
        var sdist = 0
        for (i in graph[c]!!) {
            if (i != p) {
                val dist = dfs(graph, set, i, c)
                if (dist > fdist) {
                    sdist = fdist
                    fdist = dist
                } else {
                    sdist = sdist.coerceAtLeast(dist)
                }
            }
        }
        ans = ans.coerceAtLeast(fdist + sdist)
        return 1 + fdist
    }
}
```