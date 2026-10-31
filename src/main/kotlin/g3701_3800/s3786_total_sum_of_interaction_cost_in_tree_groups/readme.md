[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3786\. Total Sum of Interaction Cost in Tree Groups

Hard

You are given an integer `n` and an undirected tree with `n` nodes numbered from 0 to `n - 1`. This is represented by a 2D array `edges` of length `n - 1`, where <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> indicates an undirected edge between nodes <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code>.

You are also given an integer array `group` of length `n`, where `group[i]` denotes the group label assigned to node `i`.

*   Two nodes `u` and `v` are considered part of the same group if `group[u] == group[v]`.
*   The **interaction cost** between `u` and `v` is defined as the number of edges on the unique path connecting them in the tree.

Return an integer denoting the **sum** of interaction costs over all **unordered** pairs `(u, v)` with `u != v` such that `group[u] == group[v]`.

**Example 1:**

**Input:** n = 3, edges = \[\[0,1],[1,2]], group = [1,1,1]

**Output:** 4

**Explanation:**

**![](https://assets.leetcode.com/uploads/2025/09/24/screenshot-2025-09-24-at-50538-pm.png)**

All nodes belong to group 1. The interaction costs between the pairs of nodes are:

*   Nodes `(0, 1)`: 1
*   Nodes `(1, 2)`: 1
*   Nodes `(0, 2)`: 2

Thus, the total interaction cost is `1 + 1 + 2 = 4`.

**Example 2:**

**Input:** n = 3, edges = \[\[0,1],[1,2]], group = [3,2,3]

**Output:** 2

**Explanation:**

*   Nodes 0 and 2 belong to group 3. The interaction cost between this pair is 2.
*   Node 1 belongs to a different group and forms no valid pair. Therefore, the total interaction cost is 2.

**Example 3:**

**Input:** n = 4, edges = \[\[0,1],[0,2],[0,3]], group = [1,1,4,4]

**Output:** 3

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/09/24/screenshot-2025-09-24-at-51312-pm.png)

Nodes belonging to the same groups and their interaction costs are:

*   Group 1: Nodes `(0, 1)`: 1
*   Group 4: Nodes `(2, 3)`: 2

Thus, the total interaction cost is `1 + 2 = 3`.

**Example 4:**

**Input:** n = 2, edges = \[\[0,1]], group = [9,8]

**Output:** 0

**Explanation:**

All nodes belong to different groups and there are no valid pairs. Therefore, the total interaction cost is 0.

**Constraints:**

*   <code>1 <= n <= 10<sup>5</sup></code>
*   `edges.length == n - 1`
*   <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code>
*   <code>0 <= u<sub>i</sub>, v<sub>i</sub> <= n - 1</code>
*   `group.length == n`
*   `1 <= group[i] <= 20`
*   The input is generated such that `edges` represents a valid tree.

## Solution

```kotlin
class Solution {
    private var totalCost: Long = 0
    private lateinit var counts: Array<IntArray>
    private lateinit var totalInGroup: IntArray
    private lateinit var adj: MutableList<MutableList<Int>>

    fun interactionCosts(n: Int, edges: Array<IntArray>, group: IntArray): Long {
        adj = ArrayList<MutableList<Int>>()
        for (i in 0..<n) {
            adj.add(ArrayList<Int>())
        }
        for (edge in edges) {
            adj[edge[0]].add(edge[1])
            adj[edge[1]].add(edge[0])
        }
        totalInGroup = IntArray(21)
        for (g in group) {
            totalInGroup[g]++
        }
        counts = Array<IntArray>(n) { IntArray(21) }
        dfs(0, -1, group)
        return totalCost
    }

    private fun dfs(u: Int, p: Int, group: IntArray) {
        counts[u][group[u]] = 1
        for (v in adj[u]) {
            if (v == p) {
                continue
            }
            dfs(v, u, group)
            for (g in 1..20) {
                if (totalInGroup[g] < 2) {
                    continue
                }
                val inSubtree = counts[v][g].toLong()
                val outsideSubtree = totalInGroup[g] - inSubtree
                totalCost += inSubtree * outsideSubtree
                counts[u][g] += counts[v][g]
            }
        }
    }
}
```