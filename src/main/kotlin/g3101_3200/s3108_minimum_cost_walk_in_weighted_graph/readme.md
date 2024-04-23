[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3108\. Minimum Cost Walk in Weighted Graph

Hard

There is an undirected weighted graph with `n` vertices labeled from `0` to `n - 1`.

You are given the integer `n` and an array `edges`, where <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code> indicates that there is an edge between vertices <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code> with a weight of <code>w<sub>i</sub></code>.

A walk on a graph is a sequence of vertices and edges. The walk starts and ends with a vertex, and each edge connects the vertex that comes before it and the vertex that comes after it. It's important to note that a walk may visit the same edge or vertex more than once.

The **cost** of a walk starting at node `u` and ending at node `v` is defined as the bitwise `AND` of the weights of the edges traversed during the walk. In other words, if the sequence of edge weights encountered during the walk is <code>w<sub>0</sub>, w<sub>1</sub>, w<sub>2</sub>, ..., w<sub>k</sub></code>, then the cost is calculated as <code>w<sub>0</sub> & w<sub>1</sub> & w<sub>2</sub> & ... & w<sub>k</sub></code>, where `&` denotes the bitwise `AND` operator.

You are also given a 2D array `query`, where <code>query[i] = [s<sub>i</sub>, t<sub>i</sub>]</code>. For each query, you need to find the minimum cost of the walk starting at vertex <code>s<sub>i</sub></code> and ending at vertex <code>t<sub>i</sub></code>. If there exists no such walk, the answer is `-1`.

Return _the array_ `answer`_, where_ `answer[i]` _denotes the **minimum** cost of a walk for query_ `i`.

**Example 1:**

**Input:** n = 5, edges = \[\[0,1,7],[1,3,7],[1,2,1]], query = \[\[0,3],[3,4]]

**Output:** [1,-1]

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/01/31/q4_example1-1.png)

To achieve the cost of 1 in the first query, we need to move on the following edges: `0->1` (weight 7), `1->2` (weight 1), `2->1` (weight 1), `1->3` (weight 7).

In the second query, there is no walk between nodes 3 and 4, so the answer is -1.

**Example 2:**

**Input:** n = 3, edges = \[\[0,2,7],[0,1,15],[1,2,6],[1,2,1]], query = \[\[1,2]]

**Output:** [0]

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/01/31/q4_example2e.png)

To achieve the cost of 0 in the first query, we need to move on the following edges: `1->2` (weight 1), `2->1` (weight 6), `1->2` (weight 1).

**Constraints:**

*   <code>2 <= n <= 10<sup>5</sup></code>
*   <code>0 <= edges.length <= 10<sup>5</sup></code>
*   `edges[i].length == 3`
*   <code>0 <= u<sub>i</sub>, v<sub>i</sub> <= n - 1</code>
*   <code>u<sub>i</sub> != v<sub>i</sub></code>
*   <code>0 <= w<sub>i</sub> <= 10<sup>5</sup></code>
*   <code>1 <= query.length <= 10<sup>5</sup></code>
*   `query[i].length == 2`
*   <code>0 <= s<sub>i</sub>, t<sub>i</sub> <= n - 1</code>
*   <code>s<sub>i</sub> != t<sub>i</sub></code>

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun minimumCost(n: Int, edges: Array<IntArray>, query: Array<IntArray>): IntArray {
        val parent = IntArray(n)
        val bitwise = IntArray(n)
        val size = IntArray(n)
        var i = 0
        while (i < n) {
            parent[i] = i
            size[i] = 1
            bitwise[i] = -1
            i++
        }
        val len = edges.size
        i = 0
        while (i < len) {
            val node1 = edges[i][0]
            val node2 = edges[i][1]
            val weight = edges[i][2]
            val parent1 = findParent(node1, parent)
            val parent2 = findParent(node2, parent)
            if (parent1 == parent2) {
                bitwise[parent1] = bitwise[parent1] and weight
            } else {
                var bitwiseVal: Int
                val check1 = bitwise[parent1] == -1
                val check2 = bitwise[parent2] == -1
                bitwiseVal = if (check1 && check2) {
                    weight
                } else if (check1) {
                    weight and bitwise[parent2]
                } else if (check2) {
                    weight and bitwise[parent1]
                } else {
                    weight and bitwise[parent1] and bitwise[parent2]
                }
                if (size[parent1] >= size[parent2]) {
                    parent[parent2] = parent1
                    size[parent1] += size[parent2]
                    bitwise[parent1] = bitwiseVal
                } else {
                    parent[parent1] = parent2
                    size[parent2] += size[parent1]
                    bitwise[parent2] = bitwiseVal
                }
            }
            i++
        }
        val queryLen = query.size
        val result = IntArray(queryLen)
        i = 0
        while (i < queryLen) {
            val start = query[i][0]
            val end = query[i][1]
            val parentStart = findParent(start, parent)
            val parentEnd = findParent(end, parent)
            if (start == end) {
                result[i] = 0
            } else if (parentStart == parentEnd) {
                result[i] = bitwise[parentStart]
            } else {
                result[i] = -1
            }
            i++
        }
        return result
    }

    private fun findParent(node: Int, parent: IntArray): Int {
        var node = node
        while (parent[node] != node) {
            node = parent[node]
        }
        return node
    }
}
```