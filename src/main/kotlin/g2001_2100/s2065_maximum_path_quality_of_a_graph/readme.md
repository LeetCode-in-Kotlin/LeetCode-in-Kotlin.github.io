[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2065\. Maximum Path Quality of a Graph

Hard

There is an **undirected** graph with `n` nodes numbered from `0` to `n - 1` (**inclusive**). You are given a **0-indexed** integer array `values` where `values[i]` is the **value** of the <code>i<sup>th</sup></code> node. You are also given a **0-indexed** 2D integer array `edges`, where each <code>edges[j] = [u<sub>j</sub>, v<sub>j</sub>, time<sub>j</sub>]</code> indicates that there is an undirected edge between the nodes <code>u<sub>j</sub></code> and <code>v<sub>j</sub></code>, and it takes <code>time<sub>j</sub></code> seconds to travel between the two nodes. Finally, you are given an integer `maxTime`.

A **valid** **path** in the graph is any path that starts at node `0`, ends at node `0`, and takes **at most** `maxTime` seconds to complete. You may visit the same node multiple times. The **quality** of a valid path is the **sum** of the values of the **unique nodes** visited in the path (each node's value is added **at most once** to the sum).

Return _the **maximum** quality of a valid path_.

**Note:** There are **at most four** edges connected to each node.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/10/19/ex1drawio.png)

**Input:** values = [0,32,10,43], edges = \[\[0,1,10],[1,2,15],[0,3,10]], maxTime = 49

**Output:** 75

**Explanation:** 

One possible path is 0 -> 1 -> 0 -> 3 -> 0.

The total time taken is 10 + 10 + 10 + 10 = 40 <= 49. 

The nodes visited are 0, 1, and 3, giving a maximal path quality of 0 + 32 + 43 = 75.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/10/19/ex2drawio.png)

**Input:** values = [5,10,15,20], edges = \[\[0,1,10],[1,2,10],[0,3,10]], maxTime = 30

**Output:** 25

**Explanation:**

One possible path is 0 -> 3 -> 0. 

The total time taken is 10 + 10 = 20 <= 30.

The nodes visited are 0 and 3, giving a maximal path quality of 5 + 20 = 25.

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/10/19/ex31drawio.png)

**Input:** values = [1,2,3,4], edges = \[\[0,1,10],[1,2,11],[2,3,12],[1,3,13]], maxTime = 50

**Output:** 7

**Explanation:** 

One possible path is 0 -> 1 -> 3 -> 1 -> 0. 

The total time taken is 10 + 13 + 13 + 10 = 46 <= 50. 

The nodes visited are 0, 1, and 3, giving a maximal path quality of 1 + 2 + 4 = 7.

**Constraints:**

*   `n == values.length`
*   `1 <= n <= 1000`
*   <code>0 <= values[i] <= 10<sup>8</sup></code>
*   `0 <= edges.length <= 2000`
*   `edges[j].length == 3`
*   <code>0 <= u<sub>j</sub> < v<sub>j</sub> <= n - 1</code>
*   <code>10 <= time<sub>j</sub>, maxTime <= 100</code>
*   All the pairs <code>[u<sub>j</sub>, v<sub>j</sub>]</code> are **unique**.
*   There are **at most four** edges connected to each node.
*   The graph may not be connected.

## Solution

```kotlin
class Solution {
    private var maxQuality = 0

    internal class Node(var i: Int, var time: Int)

    fun maximalPathQuality(values: IntArray, edges: Array<IntArray>, maxTime: Int): Int {
        val graph: MutableList<MutableList<Node>> = ArrayList()
        for (i in values.indices) {
            graph.add(ArrayList())
        }
        for (edge in edges) {
            val u = edge[0]
            val v = edge[1]
            val time = edge[2]
            val node1 = Node(u, time)
            val node2 = Node(v, time)
            graph[u].add(node2)
            graph[v].add(node1)
        }
        maxQuality = 0
        dfs(graph, 0, 0, maxTime, values[0], values)
        return maxQuality
    }

    private fun dfs(
        graph: List<MutableList<Node>>,
        start: Int,
        curTime: Int,
        maxTime: Int,
        curValue: Int,
        values: IntArray,
    ) {
        if (curTime > maxTime) {
            return
        }
        if (curTime == maxTime && start != 0) {
            return
        }
        if (start == 0) {
            maxQuality = maxQuality.coerceAtLeast(curValue)
        }
        val tmp = values[start]
        if (tmp != 0) {
            values[start] = 0
        }
        for (node in graph[start]) {
            val v = node.i
            val time = node.time
            val value = values[v]
            if (value != 0) {
                values[v] = 0
            }
            dfs(graph, v, curTime + time, maxTime, curValue + value, values)
            if (value != 0) {
                values[v] = value
            }
        }
        if (tmp != 0) {
            values[start] = tmp
        }
    }
}
```