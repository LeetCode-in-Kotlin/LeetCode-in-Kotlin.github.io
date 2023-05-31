[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1042\. Flower Planting With No Adjacent

Medium

You have `n` gardens, labeled from `1` to `n`, and an array `paths` where <code>paths[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> describes a bidirectional path between garden <code>x<sub>i</sub></code> to garden <code>y<sub>i</sub></code>. In each garden, you want to plant one of 4 types of flowers.

All gardens have **at most 3** paths coming into or leaving it.

Your task is to choose a flower type for each garden such that, for any two gardens connected by a path, they have different types of flowers.

Return _**any** such a choice as an array_ `answer`_, where_ `answer[i]` _is the type of flower planted in the_ <code>(i+1)<sup>th</sup></code> _garden. The flower types are denoted_ `1`_,_ `2`_,_ `3`_, or_ `4`_. It is guaranteed an answer exists._

**Example 1:**

**Input:** n = 3, paths = \[\[1,2],[2,3],[3,1]]

**Output:** [1,2,3]

**Explanation:** 

Gardens 1 and 2 have different types. 

Gardens 2 and 3 have different types. 

Gardens 3 and 1 have different types. 

Hence, [1,2,3] is a valid answer. Other valid answers include [1,2,4], [1,4,2], and [3,2,1].

**Example 2:**

**Input:** n = 4, paths = \[\[1,2],[3,4]]

**Output:** [1,2,1,2]

**Example 3:**

**Input:** n = 4, paths = \[\[1,2],[2,3],[3,4],[4,1],[1,3],[2,4]]

**Output:** [1,2,3,4]

**Constraints:**

*   <code>1 <= n <= 10<sup>4</sup></code>
*   <code>0 <= paths.length <= 2 * 10<sup>4</sup></code>
*   `paths[i].length == 2`
*   <code>1 <= x<sub>i</sub>, y<sub>i</sub> <= n</code>
*   <code>x<sub>i</sub> != y<sub>i</sub></code>
*   Every garden has **at most 3** paths coming into or leaving it.

## Solution

```kotlin
class Solution {
    private lateinit var graph: Array<ArrayList<Int>?>
    private lateinit var color: IntArray
    private lateinit var visited: BooleanArray

    fun gardenNoAdj(n: Int, paths: Array<IntArray>): IntArray {
        buildGraph(n, paths)
        color = IntArray(n)
        visited = BooleanArray(n)
        for (i in 0 until n) {
            if (!visited[i]) {
                dfs(i)
            }
        }
        return color
    }

    private fun dfs(at: Int) {
        visited[at] = true
        var used = 0
        for (to in graph[at]!!) {
            if (color[to] != 0) {
                used = used or (1 shl color[to] - 1)
            }
        }

        // use available color
        for (i in 0..3) {
            if (used and (1 shl i) == 0) {
                color[at] = i + 1
                break
            }
        }
    }

    private fun buildGraph(n: Int, paths: Array<IntArray>) {
        graph = arrayOfNulls(n)
        for (i in 0 until n) {
            graph[i] = ArrayList()
        }
        for (path in paths) {
            val u = path[0] - 1
            val v = path[1] - 1
            graph[u]!!.add(v)
            graph[v]!!.add(u)
        }
    }
}
```