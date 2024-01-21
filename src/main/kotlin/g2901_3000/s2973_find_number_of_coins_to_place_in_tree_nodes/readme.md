[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2973\. Find Number of Coins to Place in Tree Nodes

Hard

You are given an **undirected** tree with `n` nodes labeled from `0` to `n - 1`, and rooted at node `0`. You are given a 2D integer array `edges` of length `n - 1`, where <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> indicates that there is an edge between nodes <code>a<sub>i</sub></code> and <code>b<sub>i</sub></code> in the tree.

You are also given a **0-indexed** integer array `cost` of length `n`, where `cost[i]` is the **cost** assigned to the <code>i<sup>th</sup></code> node.

You need to place some coins on every node of the tree. The number of coins to be placed at node `i` can be calculated as:

*   If size of the subtree of node `i` is less than `3`, place `1` coin.
*   Otherwise, place an amount of coins equal to the **maximum** product of cost values assigned to `3` distinct nodes in the subtree of node `i`. If this product is **negative**, place `0` coins.

Return _an array_ `coin` _of size_ `n` _such that_ `coin[i]` _is the number of coins placed at node_ `i`_._

**Example 1:**

![](https://assets.leetcode.com/uploads/2023/11/09/screenshot-2023-11-10-012641.png)

**Input:** edges = \[\[0,1],[0,2],[0,3],[0,4],[0,5]], cost = [1,2,3,4,5,6]

**Output:** [120,1,1,1,1,1]

**Explanation:** For node 0 place 6 \* 5 \* 4 = 120 coins. All other nodes are leaves with subtree of size 1, place 1 coin on each of them.

**Example 2:**

![](https://assets.leetcode.com/uploads/2023/11/09/screenshot-2023-11-10-012614.png)

**Input:** edges = \[\[0,1],[0,2],[1,3],[1,4],[1,5],[2,6],[2,7],[2,8]], cost = [1,4,2,3,5,7,8,-4,2]

**Output:** [280,140,32,1,1,1,1,1,1]

**Explanation:** The coins placed on each node are: - Place 8 \* 7 \* 5 = 280 coins on node 0. - Place 7 \* 5 \* 4 = 140 coins on node 1. - Place 8 \* 2 \* 2 = 32 coins on node 2. - All other nodes are leaves with subtree of size 1, place 1 coin on each of them.

**Example 3:**

![](https://assets.leetcode.com/uploads/2023/11/09/screenshot-2023-11-10-012513.png)

**Input:** edges = \[\[0,1],[0,2]], cost = [1,2,-2]

**Output:** [0,1,1]

**Explanation:** Node 1 and 2 are leaves with subtree of size 1, place 1 coin on each of them. For node 0 the only possible product of cost is 2 \* 1 \* -2 = -4. Hence place 0 coins on node 0.

**Constraints:**

*   <code>2 <= n <= 2 * 10<sup>4</sup></code>
*   `edges.length == n - 1`
*   `edges[i].length == 2`
*   <code>0 <= a<sub>i</sub>, b<sub>i</sub> < n</code>
*   `cost.length == n`
*   <code>1 <= |cost[i]| <= 10<sup>4</sup></code>
*   The input is generated such that `edges` represents a valid tree.

## Solution

```kotlin
import java.util.PriorityQueue
import kotlin.math.max

class Solution {
    private lateinit var result: LongArray

    fun placedCoins(edges: Array<IntArray>, cost: IntArray): LongArray {
        val n = cost.size
        val g: MutableList<MutableList<Int>> = ArrayList()
        for (i in 0 until n) {
            g.add(ArrayList())
        }
        for (e in edges) {
            g[e[0]].add(e[1])
            g[e[1]].add(e[0])
        }
        result = LongArray(n)
        dp(g, cost, 0, -1)
        return result
    }

    private class PQX {
        var min: PriorityQueue<Int>? = null
        var max: PriorityQueue<Int>? = null
    }

    private fun dp(g: List<MutableList<Int>>, cost: IntArray, i: Int, p: Int): PQX {
        if (i >= g.size) {
            val pqx = PQX()
            pqx.max = PriorityQueue { a: Int, b: Int -> b - a }
            pqx.min = PriorityQueue(Comparator.comparingInt { a: Int? -> a!! })
            return pqx
        }
        val next: List<Int> = g[i]
        var pq = PriorityQueue { a: Int, b: Int -> b - a }
        var pq2 = PriorityQueue(Comparator.comparingInt { a: Int? -> a!! })
        if (cost[i] > 0) {
            pq.add(cost[i])
        } else {
            pq2.add(cost[i])
        }
        for (ne in next) {
            if (ne != p) {
                val r = dp(g, cost, ne, i)
                while (r.min!!.isNotEmpty()) {
                    val a = r.min!!.poll()
                    pq2.add(a)
                }
                while (r.max!!.isNotEmpty()) {
                    val a = r.max!!.poll()
                    pq.add(a)
                }
            }
        }
        if (pq.size + pq2.size < 3) {
            result[i] = 1
        } else {
            val a = if (pq.isNotEmpty()) pq.poll() else 0
            val b = if (pq.isNotEmpty()) pq.poll() else 0
            val c = if (pq.isNotEmpty()) pq.poll() else 0
            val aa = if (pq2.isNotEmpty()) pq2.poll() else 0
            val bb = if (pq2.isNotEmpty()) pq2.poll() else 0
            result[i] = max(0, (a.toLong() * b * c))
            result[i] = max(result[i], max(0, (a.toLong() * aa * bb)))
                .toLong()
            pq = PriorityQueue { x: Int, y: Int -> y - x }
            pq.add(a)
            pq.add(b)
            pq.add(c)
            pq2 = PriorityQueue(Comparator.comparingInt { x: Int? -> x!! })
            pq2.add(aa)
            pq2.add(bb)
        }
        val pqx = PQX()
        pqx.min = pq2
        pqx.max = pq
        return pqx
    }
}
```