[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3928\. Minimum Cost to Buy Apples II

Hard

You are given an integer `n` and an integer array `prices` of length `n`, where `prices[i]` is the price of apples at shop `i`.

You are also given a 2D integer array `roads`, where <code>roads[i] = [u<sub>i</sub>, v<sub>i</sub>, cost<sub>i</sub>, tax<sub>i</sub>]</code> represents a **bidirectional** road:

*   <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code> are the shops connected by the road.
*   <code>cost<sub>i</sub></code> is the cost to travel the road **without** carrying apples.
*   <code>tax<sub>i</sub></code> is the multiplier applied to <code>cost<sub>i</sub></code> when traveling **with** apples.

For each shop `i`, you can either:

*   Buy apples locally at shop `i` for `prices[i]`.
*   Travel **empty** to any shop `j` using **any** number of roads, buy apples for `prices[j]`, and return to shop `i` while carrying apples, paying `cost * tax` on each road used for the return trip.

The forward path, where you travel empty, and the return path may be **different**.

Return an integer array `ans` of length `n`, where `ans[i]` is the **minimum** total cost to buy apples starting from shop `i`.

**Example 1:**

**Input:** n = 2, prices = [8,3], roads = \[\[0,1,1,2]]

**Output:** [6,3]

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/08/22/screenshot-2025-08-23-at-23341-am.png)

| Shop `i` | `prices[i]` | Shop `j` | `prices[j]` | `costᵢ` | `taxᵢ` | Travel cost | Return cost |            Total |          Minimum |
| -------: | ----------: | -------: | ----------: | ------: | -----: | ----------: | ----------: | ---------------: | ---------------: |
|        0 |           8 |        1 |           3 |       1 |      2 |           1 | `1 * 2 = 2` |  `1 + 2 + 3 = 6` |  `min(8, 6) = 6` |
|        1 |           3 |        0 |           8 |       1 |      2 |           1 | `1 * 2 = 2` | `1 + 2 + 8 = 11` | `min(3, 11) = 3` |

Thus, the answer is `[6, 3]`.

**Example 2:**

**Input:** n = 3, prices = [9,4,6], roads = \[\[0,1,1,3],[1,2,4,2]]

**Output:** [8,4,6]

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/08/22/screenshot-2025-08-23-at-23736-am.png)

| Shop `i` | `prices[i]` | Shop `j` | `prices[j]` | `costᵢ` | `taxᵢ` | Travel cost | Return cost |            Total |          Minimum |
| -------: | ----------: | -------: | ----------: | ------: | -----: | ----------: | ----------: | ---------------: | ---------------: |
|        0 |           9 |        1 |           4 |       1 |      3 |           1 | `1 * 3 = 3` |  `1 + 3 + 4 = 8` |  `min(9, 8) = 8` |
|        1 |           4 |        2 |           6 |       4 |      2 |           4 | `4 * 2 = 8` | `4 + 8 + 6 = 18` | `min(4, 18) = 4` |
|        2 |           6 |        1 |           4 |       4 |      2 |           4 | `4 * 2 = 8` | `4 + 8 + 4 = 16` | `min(6, 16) = 6` |

Thus, the answer is `[8, 4, 6]`.

**Example 3:**

**Input:** n = 3, prices = [10,11,1], roads = \[\[0,2,1,3],[1,2,3,4],[0,1,5,2]]

**Output:** [5,11,1]

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/08/22/screenshot-2025-08-23-at-24644-am.png)

| Shop `i` | `prices[i]` | Shop `j` | `prices[j]` | `costᵢ` | `taxᵢ` | Travel cost |  Return cost |             Total |            Minimum |
| -------: | ----------: | -------: | ----------: | ------: | -----: | ----------: | -----------: | ----------------: | -----------------: |
|        0 |          10 |        2 |           1 |       1 |      3 |           1 |  `1 * 3 = 3` |   `1 + 3 + 1 = 5` |   `min(10, 5) = 5` |
|        1 |          11 |        2 |           1 |       3 |      4 |           3 | `3 * 4 = 12` | `3 + 12 + 1 = 16` | `min(11, 16) = 11` |
|        2 |           1 |        0 |          10 |       1 |      3 |           1 |  `1 * 3 = 3` | `1 + 3 + 10 = 14` |   `min(1, 14) = 1` |

Thus, the answer is `[5, 11, 1]`.

**Constraints:**

*   `1 <= n <= 1000`
*   `prices.length == n`
*   <code>1 <= prices[i] <= 10<sup>9</sup></code>
*   `0 <= roads.length <= min(n × (n - 1) / 2, 2000)`
*   <code>roads[i] = [u<sub>i</sub>, v<sub>i</sub>, cost<sub>i</sub>, tax<sub>i</sub>]</code>
*   <code>0 <= u<sub>i</sub>, v<sub>i</sub> <= n - 1</code>
*   <code>u<sub>i</sub> != v<sub>i</sub></code>
*   <code>1 <= cost<sub>i</sub> <= 10<sup>9</sup></code>
*   <code>1 <= tax<sub>i</sub> <= 100</code>
*   There are no **repeated** edges.

## Solution

```kotlin
import java.util.PriorityQueue

class Solution {
    fun minCost(n: Int, prices: IntArray, roads: Array<IntArray>): IntArray {
        val adj = adjacency(n, roads)
        val ans = prices.copyOf(n)
        for (i in 0..<n) {
            val forward = shortestPath(adj, i, prices[i], true)
            val backward = shortestPath(adj, i, prices[i], false)
            for (j in 0..<n) {
                if (j != i && forward[j] != Long.MAX_VALUE && backward[j] != Long.MAX_VALUE) {
                    val d = forward[j] + prices[j] + backward[j]
                    if (d < ans[i]) {
                        ans[i] = d.toInt()
                    }
                }
            }
        }
        return ans
    }

    private fun shortestPath(
        adj: MutableList<MutableList<Edge>?>,
        src: Int,
        maxPrice: Int,
        forward: Boolean,
    ): LongArray {
        val dist = LongArray(adj.size)
        dist.fill(Long.MAX_VALUE)
        dist[src] = 0L
        val pq = PriorityQueue<Distance>()
        pq.offer(Distance(src, dist[src]))
        while (pq.isNotEmpty() && pq.peek().dist <= maxPrice) {
            val distance = pq.poll()
            if (distance.dist > dist[distance.node]) {
                continue
            }
            for (edge in adj[distance.node]!!) {
                val d = if (forward) edge.forward else edge.backward
                if (dist[edge.node] > dist[distance.node] + d) {
                    dist[edge.node] = dist[distance.node] + d
                    pq.offer(Distance(edge.node, dist[edge.node]))
                }
            }
        }
        return dist
    }

    private fun adjacency(n: Int, roads: Array<IntArray>): MutableList<MutableList<Edge>?> {
        val adj: MutableList<MutableList<Edge>?> = ArrayList<MutableList<Edge>?>()
        for (i in 0..<n) {
            adj.add(ArrayList<Edge>())
        }
        for (r in roads) {
            val f = r[2].toLong()
            val b = f * r[3]
            adj[r[0]]!!.add(Edge(r[1], f, b))
            adj[r[1]]!!.add(Edge(r[0], f, b))
        }
        return adj
    }

    private class Edge(val node: Int, val forward: Long, val backward: Long)

    private class Distance(val node: Int, val dist: Long) : Comparable<Distance> {
        override fun compareTo(other: Distance): Int = this.dist.compareTo(other.dist)
    }
}
```