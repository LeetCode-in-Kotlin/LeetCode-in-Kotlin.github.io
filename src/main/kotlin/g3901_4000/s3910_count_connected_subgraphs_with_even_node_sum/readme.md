[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3910\. Count Connected Subgraphs with Even Node Sum

Hard

You are given an undirected graph with `n` nodes labeled from 0 to `n - 1`. Node `i` has a **value** of `nums[i]`, which is either 0 or 1. The edges of the graph are given by a 2D array `edges` where <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> represents an edge between node <code>u<sub>i</sub></code> and node <code>v<sub>i</sub></code>.

For a **non-empty subset** `s` of nodes in the graph, we consider the **induced subgraph** of `s` generated as follows:

*   We keep only the nodes in `s`.
*   We keep only the edges whose two endpoints are both in `s`.

Return an integer representing the number of **non-empty** subsets `s` of nodes in the graph such that:

*   The **induced subgraph** of `s` is **connected**.
*   The **sum** of node **values** in `s` is **even**.

**Example 1:**

**Input:** nums = [1,0,1], edges = \[\[0,1],[1,2]]

**Output:** 2

**Explanation:**

| `s` | connected? | sum of node values | counted? |
|---|---|---:|---|
| `[0]` | Yes | 1 | No |
| `[1]` | Yes | 0 | Yes |
| `[2]` | Yes | 1 | No |
| `[0,1]` | Yes | 1 | No |
| `[0,2]` | No, node 0 and node 2 are disconnected. | 2 | No |
| `[1,2]` | Yes | 1 | No |
| `[0,1,2]` | Yes | 2 | Yes |

**Example 2:**

**Input:** nums = [1], edges = []

**Output:** 0

**Explanation:**

| `s` | connected? | sum of node values | counted? |
|---|---|---:|---|
| `[0]` | Yes | 1 | No |

**Constraints:**

*   `1 <= n == nums.length <= 13`
*   `nums[i]` is 0 or 1.
*   `0 <= edges.length <= n * (n - 1) / 2`
*   <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code>
*   <code>0 <= u<sub>i</sub> < v<sub>i</sub> < n</code>
*   All edges are **distinct**.

## Solution

```kotlin
class Solution {
    private lateinit var graph: LongArray
    private lateinit var nums: IntArray
    private var validCount = 0

    fun evenSumSubgraphs(nums: IntArray, edges: Array<IntArray>): Int {
        this.nums = nums
        val nodeCount = nums.size
        this.graph = LongArray(nodeCount)
        this.validCount = 0
        buildGraph(edges)
        for (root in 0..<nodeCount) {
            val rootMask = 1L shl root
            val allowedMask = -(1L shl root)
            val candidateMask = graph[root] and allowedMask
            search(rootMask, candidateMask, 0L, nums[root] and 1, allowedMask)
        }
        return validCount
    }

    private fun buildGraph(edgeList: Array<IntArray>) {
        for (edge in edgeList) {
            val firstNode = edge[0]
            val secondNode = edge[1]
            graph[firstNode] = graph[firstNode] or (1L shl secondNode)
            graph[secondNode] = graph[secondNode] or (1L shl firstNode)
        }
    }

    private fun search(selectedMask: Long, candidateMask: Long, excludedMask: Long, parity: Int, allowedMask: Long) {
        var candidateMask = candidateMask
        var excludedMask = excludedMask
        if (parity == 0) {
            validCount++
        }
        while (candidateMask != 0L) {
            val currentBit = candidateMask and -candidateMask
            val currentNode = java.lang.Long.numberOfTrailingZeros(currentBit)
            candidateMask = candidateMask xor currentBit
            val nextSelectedMask = selectedMask or currentBit
            val nextCandidateMask =
                (
                    candidateMask
                        or (
                            graph[currentNode]
                                and allowedMask
                                and nextSelectedMask.inv()
                                and excludedMask.inv()
                            )
                    )
            search(
                nextSelectedMask,
                nextCandidateMask,
                excludedMask,
                parity xor (nums[currentNode] and 1),
                allowedMask,
            )
            excludedMask = excludedMask or currentBit
        }
    }
}
```