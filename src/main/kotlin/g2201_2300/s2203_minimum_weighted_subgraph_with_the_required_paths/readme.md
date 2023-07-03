[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2203\. Minimum Weighted Subgraph With the Required Paths

Hard

You are given an integer `n` denoting the number of nodes of a **weighted directed** graph. The nodes are numbered from `0` to `n - 1`.

You are also given a 2D integer array `edges` where <code>edges[i] = [from<sub>i</sub>, to<sub>i</sub>, weight<sub>i</sub>]</code> denotes that there exists a **directed** edge from <code>from<sub>i</sub></code> to <code>to<sub>i</sub></code> with weight <code>weight<sub>i</sub></code>.

Lastly, you are given three **distinct** integers `src1`, `src2`, and `dest` denoting three distinct nodes of the graph.

Return _the **minimum weight** of a subgraph of the graph such that it is **possible** to reach_ `dest` _from both_ `src1` _and_ `src2` _via a set of edges of this subgraph_. In case such a subgraph does not exist, return `-1`.

A **subgraph** is a graph whose vertices and edges are subsets of the original graph. The **weight** of a subgraph is the sum of weights of its constituent edges.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/02/17/example1drawio.png)

**Input:** n = 6, edges = \[\[0,2,2],[0,5,6],[1,0,3],[1,4,5],[2,1,1],[2,3,3],[2,3,4],[3,4,2],[4,5,1]], src1 = 0, src2 = 1, dest = 5

**Output:** 9

**Explanation:** The above figure represents the input graph. The blue edges represent one of the subgraphs that yield the optimal answer. Note that the subgraph [[1,0,3],[0,5,6]] also yields the optimal answer. It is not possible to get a subgraph with less weight satisfying all the constraints.

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/02/17/example2-1drawio.png)

**Input:** n = 3, edges = \[\[0,1,1],[2,1,1]], src1 = 0, src2 = 1, dest = 2

**Output:** -1

**Explanation:** The above figure represents the input graph. It can be seen that there does not exist any path from node 1 to node 2, hence there are no subgraphs satisfying all the constraints.

**Constraints:**

*   <code>3 <= n <= 10<sup>5</sup></code>
*   <code>0 <= edges.length <= 10<sup>5</sup></code>
*   `edges[i].length == 3`
*   <code>0 <= from<sub>i</sub>, to<sub>i</sub>, src1, src2, dest <= n - 1</code>
*   <code>from<sub>i</sub> != to<sub>i</sub></code>
*   `src1`, `src2`, and `dest` are pairwise distinct.
*   <code>1 <= weight[i] <= 10<sup>5</sup></code>

## Solution

```kotlin
import java.util.PriorityQueue
import java.util.Queue

class Solution {
    fun minimumWeight(n: Int, edges: Array<IntArray>, src1: Int, src2: Int, dest: Int): Long {
        val graph: Array<MutableList<IntArray>?> = arrayOfNulls(n)
        val weight = Array(3) { LongArray(n) }
        for (i in 0 until n) {
            for (j in 0..2) {
                weight[j][i] = Long.MAX_VALUE
            }
            graph[i] = ArrayList()
        }
        for (e in edges) {
            graph[e[0]]?.add(intArrayOf(e[1], e[2]))
        }
        val queue: Queue<Node> = PriorityQueue({ node1: Node, node2: Node -> node1.weight.compareTo(node2.weight) })
        queue.offer(Node(0, src1, 0))
        weight[0][src1] = 0
        queue.offer(Node(1, src2, 0))
        weight[1][src2] = 0
        while (queue.isNotEmpty()) {
            val curr = queue.poll()
            if (curr.vertex == dest && curr.index == 2) {
                return curr.weight
            }
            for (next in graph[curr.vertex]!!) {
                if (curr.index == 2 && weight[curr.index][next[0]] > curr.weight + next[1]) {
                    weight[curr.index][next[0]] = curr.weight + next[1]
                    queue.offer(Node(curr.index, next[0], weight[curr.index][next[0]]))
                } else if (weight[curr.index][next[0]] > curr.weight + next[1]) {
                    weight[curr.index][next[0]] = curr.weight + next[1]
                    queue.offer(Node(curr.index, next[0], weight[curr.index][next[0]]))
                    if (weight[curr.index xor 1][next[0]] != Long.MAX_VALUE &&
                        weight[curr.index][next[0]] + weight[curr.index xor 1][next[0]]
                        < weight[2][next[0]]
                    ) {
                        weight[2][next[0]] = weight[curr.index][next[0]] + weight[curr.index xor 1][next[0]]
                        queue.offer(Node(2, next[0], weight[2][next[0]]))
                    }
                }
            }
        }
        return -1
    }

    private class Node(var index: Int, var vertex: Int, var weight: Long)
}
```