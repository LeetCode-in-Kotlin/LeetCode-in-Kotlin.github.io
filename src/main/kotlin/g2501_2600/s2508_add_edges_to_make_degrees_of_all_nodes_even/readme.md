[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2508\. Add Edges to Make Degrees of All Nodes Even

Hard

There is an **undirected** graph consisting of `n` nodes numbered from `1` to `n`. You are given the integer `n` and a **2D** array `edges` where <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> indicates that there is an edge between nodes <code>a<sub>i</sub></code> and <code>b<sub>i</sub></code>. The graph can be disconnected.

You can add **at most** two additional edges (possibly none) to this graph so that there are no repeated edges and no self-loops.

Return `true` _if it is possible to make the degree of each node in the graph even, otherwise return_ `false`_._

The degree of a node is the number of edges connected to it.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/10/26/agraphdrawio.png)

**Input:** n = 5, edges = \[\[1,2],[2,3],[3,4],[4,2],[1,4],[2,5]]

**Output:** true

**Explanation:** The above diagram shows a valid way of adding an edge. Every node in the resulting graph is connected to an even number of edges.

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/10/26/aagraphdrawio.png)

**Input:** n = 4, edges = \[\[1,2],[3,4]]

**Output:** true

**Explanation:** The above diagram shows a valid way of adding two edges.

**Example 3:**

![](https://assets.leetcode.com/uploads/2022/10/26/aaagraphdrawio.png)

**Input:** n = 4, edges = \[\[1,2],[1,3],[1,4]]

**Output:** false

**Explanation:** It is not possible to obtain a valid graph with adding at most 2 edges.

**Constraints:**

*   <code>3 <= n <= 10<sup>5</sup></code>
*   <code>2 <= edges.length <= 10<sup>5</sup></code>
*   `edges[i].length == 2`
*   <code>1 <= a<sub>i</sub>, b<sub>i</sub> <= n</code>
*   <code>a<sub>i</sub> != b<sub>i</sub></code>
*   There are no repeated edges.

## Solution

```kotlin
class Solution {
    fun isPossible(n: Int, edges: List<List<Int>>): Boolean {
        val g: Array<ArrayList<Int>?> = arrayOfNulls(n + 1)
        val oddList = ArrayList<Int>()
        for (i in 1..n) {
            g[i] = ArrayList()
        }
        for (edge in edges) {
            val x = edge[0]
            val y = edge[1]
            g[x]?.add(y)
            g[y]?.add(x)
        }
        for (i in 1..n) {
            if (g[i]!!.size % 2 == 1) {
                oddList.add(i)
            }
        }
        val size = oddList.size
        if (size == 0) {
            return true
        }
        if (size == 1 || size == 3 || size > 4) {
            return false
        }
        if (size == 2) {
            val x = oddList[0]
            val y = oddList[1]
            if (isNotConnected(x, y, g)) {
                return true
            }
            for (i in 1..n) {
                if (isNotConnected(i, x, g) && isNotConnected(i, y, g)) {
                    return true
                }
            }
            return false
        }
        val a = oddList[0]
        val b = oddList[1]
        val c = oddList[2]
        val d = oddList[3]
        if (isNotConnected(a, b, g) && isNotConnected(c, d, g)) {
            return true
        }
        return if (isNotConnected(a, c, g) && isNotConnected(b, d, g)) {
            true
        } else {
            isNotConnected(a, d, g) && isNotConnected(b, c, g)
        }
    }

    private fun isNotConnected(x: Int, y: Int, g: Array<ArrayList<Int>?>): Boolean {
        return !g[x]?.contains(y)!!
    }
}
```