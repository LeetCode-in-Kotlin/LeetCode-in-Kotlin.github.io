[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 797\. All Paths From Source to Target

Medium

Given a directed acyclic graph (**DAG**) of `n` nodes labeled from `0` to `n - 1`, find all possible paths from node `0` to node `n - 1` and return them in **any order**.

The graph is given as follows: `graph[i]` is a list of all nodes you can visit from node `i` (i.e., there is a directed edge from node `i` to node `graph[i][j]`).

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/28/all_1.jpg)

**Input:** graph = \[\[1,2],[3],[3],[]]

**Output:** [[0,1,3],[0,2,3]]

**Explanation:** There are two paths: 0 -> 1 -> 3 and 0 -> 2 -> 3.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/09/28/all_2.jpg)

**Input:** graph = \[\[4,3,1],[3,2,4],[3],[4],[]]

**Output:** [[0,4],[0,3,4],[0,1,3,4],[0,1,2,3,4],[0,1,4]]

**Constraints:**

*   `n == graph.length`
*   `2 <= n <= 15`
*   `0 <= graph[i][j] < n`
*   `graph[i][j] != i` (i.e., there will be no self-loops).
*   All the elements of `graph[i]` are **unique**.
*   The input graph is **guaranteed** to be a **DAG**.

## Solution

```kotlin
class Solution {
    private var res: MutableList<List<Int>>? = null
    fun allPathsSourceTarget(graph: Array<IntArray>): List<List<Int>> {
        res = ArrayList()
        val temp: MutableList<Int> = ArrayList()
        temp.add(0)
        // perform DFS
        solve(graph, temp, 0)
        return res as ArrayList<List<Int>>
    }

    private fun solve(graph: Array<IntArray>, temp: MutableList<Int>, lastNode: Int) {
        if (lastNode == graph.size - 1) {
            res!!.add(ArrayList(temp))
        }
        for (link in graph[lastNode]) {
            temp.add(link)
            solve(graph, temp, link)
            temp.removeAt(temp.size - 1)
        }
    }
}
```