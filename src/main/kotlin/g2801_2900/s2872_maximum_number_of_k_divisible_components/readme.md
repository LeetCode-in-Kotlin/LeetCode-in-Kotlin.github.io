[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2872\. Maximum Number of K-Divisible Components

Hard

There is an undirected tree with `n` nodes labeled from `0` to `n - 1`. You are given the integer `n` and a 2D integer array `edges` of length `n - 1`, where <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> indicates that there is an edge between nodes <code>a<sub>i</sub></code> and <code>b<sub>i</sub></code> in the tree.

You are also given a **0-indexed** integer array `values` of length `n`, where `values[i]` is the **value** associated with the <code>i<sup>th</sup></code> node, and an integer `k`.

A **valid split** of the tree is obtained by removing any set of edges, possibly empty, from the tree such that the resulting components all have values that are divisible by `k`, where the **value of a connected component** is the sum of the values of its nodes.

Return _the **maximum number of components** in any valid split_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2023/08/07/example12-cropped2svg.jpg)

**Input:** n = 5, edges = \[\[0,2],[1,2],[1,3],[2,4]], values = [1,8,1,4,4], k = 6

**Output:** 2

**Explanation:** We remove the edge connecting node 1 with 2. The resulting split is valid because: 
- The value of the component containing nodes 1 and 3 is values[1] + values[3] = 12.
- The value of the component containing nodes 0, 2, and 4 is values[0] + values[2] + values[4] = 6. 

It can be shown that no other valid split has more than 2 connected components.

**Example 2:**

![](https://assets.leetcode.com/uploads/2023/08/07/example21svg-1.jpg)

**Input:** n = 7, edges = \[\[0,1],[0,2],[1,3],[1,4],[2,5],[2,6]], values = [3,0,6,1,5,2,1], k = 3

**Output:** 3

**Explanation:** We remove the edge connecting node 0 with 2, and the edge connecting node 0 with 1. The resulting split is valid because: 
- The value of the component containing node 0 is values[0] = 3. 
- The value of the component containing nodes 2, 5, and 6 is values[2] + values[5] + values[6] = 9. 
- The value of the component containing nodes 1, 3, and 4 is values[1] + values[3] + values[4] = 6. 

It can be shown that no other valid split has more than 3 connected components.

**Constraints:**

*   <code>1 <= n <= 3 * 10<sup>4</sup></code>
*   `edges.length == n - 1`
*   `edges[i].length == 2`
*   <code>0 <= a<sub>i</sub>, b<sub>i</sub> < n</code>
*   `values.length == n`
*   <code>0 <= values[i] <= 10<sup>9</sup></code>
*   <code>1 <= k <= 10<sup>9</sup></code>
*   Sum of `values` is divisible by `k`.
*   The input is generated such that `edges` represents a valid tree.

## Solution

```kotlin
class Solution {
    private var ans = 0

    fun maxKDivisibleComponents(n: Int, edges: Array<IntArray>, values: IntArray, k: Int): Int {
        val adj: MutableList<MutableList<Int>> = ArrayList()
        for (i in 0 until n) {
            adj.add(ArrayList())
        }
        for (edge in edges) {
            val start = edge[0]
            val end = edge[1]
            adj[start].add(end)
            adj[end].add(start)
        }
        val isVis = BooleanArray(n)
        isVis[0] = true
        get(0, -1, adj, isVis, values, k.toLong())
        return ans
    }

    private fun get(
        curNode: Int,
        parent: Int,
        adj: List<MutableList<Int>>,
        isVis: BooleanArray,
        values: IntArray,
        k: Long,
    ): Long {
        var sum = values[curNode].toLong()
        for (ele in adj[curNode]) {
            if (ele != parent && !isVis[ele]) {
                isVis[ele] = true
                sum += get(ele, curNode, adj, isVis, values, k)
            }
        }
        return if (sum % k == 0L) {
            ans++
            0
        } else {
            sum
        }
    }
}
```