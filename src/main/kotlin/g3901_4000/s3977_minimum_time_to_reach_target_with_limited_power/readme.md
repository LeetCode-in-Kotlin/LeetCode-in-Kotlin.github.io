[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3977\. Minimum Time to Reach Target With Limited Power

Hard

You are given a **directed** weighted graph with `n` nodes labeled from 0 to `n - 1`.

The graph is represented by a 2D integer array `edges`, where <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, t<sub>i</sub>]</code> indicates a directed edge from node <code>u<sub>i</sub></code> to node <code>v<sub>i</sub></code> that takes <code>t<sub>i</sub></code> seconds to traverse.

You are also given an integer `power` representing the initial available power, and an integer array `cost` of length `n`, where `cost[u]` represents the power required to forward the signal from node `u` through **any** one of its outgoing edges.

You are given two integers `source` and `target`.

The signal starts at `source` at time 0 with `power` units of power and follows these rules:

*   The signal may traverse a directed edge from node `u` only if the remaining power is **at least** `cost[u]`.
*   No power is consumed when the signal arrives at a node, unless it later leaves that node by traversing another edge.
*   When the signal is forwarded from node `u`, the remaining power is **decreased** by `cost[u]` units.
*   Traversing an edge <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, t<sub>i</sub>]</code> **increases** the total time by <code>t<sub>i</sub></code> seconds.

Return an integer array `answer` of size 2, where:

*   `answer[0]` is the **minimum** time required for the signal to reach node `target`.
*   `answer[1]` is the **maximum** remaining power among all paths that achieve `answer[0]`.

If the signal cannot reach `target`, return `[-1, -1]`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2026/02/09/g1.png)

**Input:** n = 5, edges = \[\[0,1,1],[1,4,1],[0,2,1],[2,3,1],[3,4,1]], power = 4, cost = [2,3,1,1,1], source = 0, target = 4

**Output:** [3,0]

**Explanation:**

*   The signal starts at node 0 with 4 units of power.
*   The path `0 -> 1 -> 4` is not valid, because after leaving node 0, the signal has 2 units of power remaining, which is less than `cost[1] = 3`.
*   The valid path `0 -> 2 -> 3 -> 4` takes a total time of 3.
*   The total power consumed along this path is `cost[0] + cost[2] + cost[3] = 4`, leaving 0 remaining power.
*   Hence, the answer is `[3, 0]`.

**Example 2:**

![](https://assets.leetcode.com/uploads/2026/02/09/g22.png)

**Input:** n = 3, edges = \[\[0,1,2],[1,2,2],[2,0,2]], power = 3, cost = [1,1,1], source = 1, target = 1

**Output:** [0,3]

**Explanation:**

*   Since the `source` and `target` are the same node, no traversal is required.
*   Hence, the minimum total time taken is 0, and no power is consumed.
*   Therefore, the answer is `[0, 3]`.

**Example 3:**

![](https://assets.leetcode.com/uploads/2026/02/09/g23.png)

**Input:** n = 4, edges = \[\[0,1,3],[2,3,4]], power = 3, cost = [1,1,1,1], source = 0, target = 3

**Output:** [-1,-1]

**Explanation:**

There is no valid path from `source` to `target`, therefore return `[-1, -1]`.

**Constraints:**

*   `1 <= n <= 1000`
*   `0 <= edges.length <= 1000`
*   <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, t<sub>i</sub>]</code>
*   <code>0 <= u<sub>i</sub>, v<sub>i</sub> <= n - 1</code>
*   <code>1 <= t<sub>i</sub> <= 10<sup>9</sup></code>
*   `1 <= power <= 1000`
*   `cost.length == n`
*   `1 <= cost[i] <= 2000`
*   `0 <= source, target <= n - 1`

## Solution

```kotlin
class Solution {
    private class Graph(n: Int, edges: Array<IntArray>) {
        var head: IntArray
        var next: IntArray
        var to: IntArray
        var time: IntArray

        init {
            val m = edges.size
            head = IntArray(n)
            next = IntArray(m + 1)
            to = IntArray(m + 1)
            time = IntArray(m + 1)
            for (i in 0..<m) {
                val u = edges[i][0]
                to[i + 1] = edges[i][1]
                time[i + 1] = edges[i][2]
                next[i + 1] = head[u]
                head[u] = i + 1
            }
        }
    }

    private class MinHeap {
        var heap: LongArray = LongArray(2048)
        var size: Int = 0

        fun push(value: Long) {
            if (++size == heap.size) {
                heap = heap.copyOf(heap.size shl 1)
            }
            var j = size
            while (j > 1) {
                val p = j ushr 1
                if (heap[p] <= value) {
                    break
                }
                heap[j] = heap[p]
                j = p
            }
            heap[j] = value
        }

        fun pop(): Long {
            val result = heap[1]
            val `val` = heap[size--]
            var i = 1
            while ((i shl 1) <= size) {
                val left = i shl 1
                val right = left + 1
                val min = if (right <= size && heap[right] < heap[left]) right else left
                if (`val` <= heap[min]) {
                    break
                }
                heap[i] = heap[min]
                i = min
            }
            if (size > 0) {
                heap[i] = `val`
            }
            return result
        }

        val isEmpty: Boolean
            get() = size == 0
    }

    fun minTimeMaxPower(
        n: Int,
        edges: Array<IntArray>,
        power: Int,
        cost: IntArray,
        source: Int,
        target: Int,
    ): LongArray {
        val graph = Graph(n, edges)
        val bestPower = IntArray(n)
        bestPower.fill(-1)
        val pq = MinHeap()
        val startPowerComp = 1024L - power
        pq.push((startPowerComp shl 11) or source.toLong())
        while (!pq.isEmpty) {
            val curr = pq.pop()
            val currTime = curr ushr 22
            val currPower = 1024 - ((curr ushr 11) and 0x7FFL).toInt()
            val u = (curr and 0x7FFL).toInt()
            if (u == target) {
                return longArrayOf(currTime, currPower.toLong())
            }
            if (currPower > bestPower[u]) {
                bestPower[u] = currPower
                if (currPower >= cost[u]) {
                    addNeighbors(pq, currTime, currPower, u, graph, cost)
                }
            }
        }
        return longArrayOf(-1, -1)
    }

    private fun addNeighbors(pq: MinHeap, currTime: Long, currPower: Int, u: Int, graph: Graph, cost: IntArray) {
        val nextPower = currPower - cost[u]
        val nextPowerComp = 1024L - nextPower
        var e = graph.head[u]
        while (e != 0) {
            val nextPacked =
                ((currTime + graph.time[e]) shl 22) or (nextPowerComp shl 11) or graph.to[e].toLong()
            pq.push(nextPacked)
            e = graph.next[e]
        }
    }
}
```