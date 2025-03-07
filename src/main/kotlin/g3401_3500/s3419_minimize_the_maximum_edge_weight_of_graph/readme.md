[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3419\. Minimize the Maximum Edge Weight of Graph

Medium

You are given two integers, `n` and `threshold`, as well as a **directed** weighted graph of `n` nodes numbered from 0 to `n - 1`. The graph is represented by a **2D** integer array `edges`, where <code>edges[i] = [A<sub>i</sub>, B<sub>i</sub>, W<sub>i</sub>]</code> indicates that there is an edge going from node <code>A<sub>i</sub></code> to node <code>B<sub>i</sub></code> with weight <code>W<sub>i</sub></code>.

You have to remove some edges from this graph (possibly **none**), so that it satisfies the following conditions:

*   Node 0 must be reachable from all other nodes.
*   The **maximum** edge weight in the resulting graph is **minimized**.
*   Each node has **at most** `threshold` outgoing edges.

Return the **minimum** possible value of the **maximum** edge weight after removing the necessary edges. If it is impossible for all conditions to be satisfied, return -1.

**Example 1:**

**Input:** n = 5, edges = \[\[1,0,1],[2,0,2],[3,0,1],[4,3,1],[2,1,1]], threshold = 2

**Output:** 1

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/12/09/s-1.png)

Remove the edge `2 -> 0`. The maximum weight among the remaining edges is 1.

**Example 2:**

**Input:** n = 5, edges = \[\[0,1,1],[0,2,2],[0,3,1],[0,4,1],[1,2,1],[1,4,1]], threshold = 1

**Output:** \-1

**Explanation:**

It is impossible to reach node 0 from node 2.

**Example 3:**

**Input:** n = 5, edges = \[\[1,2,1],[1,3,3],[1,4,5],[2,3,2],[3,4,2],[4,0,1]], threshold = 1

**Output:** 2

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/12/09/s2-1.png)

Remove the edges `1 -> 3` and `1 -> 4`. The maximum weight among the remaining edges is 2.

**Example 4:**

**Input:** n = 5, edges = \[\[1,2,1],[1,3,3],[1,4,5],[2,3,2],[4,0,1]], threshold = 1

**Output:** \-1

**Constraints:**

*   <code>2 <= n <= 10<sup>5</sup></code>
*   `1 <= threshold <= n - 1`
*   <code>1 <= edges.length <= min(10<sup>5</sup>, n * (n - 1) / 2).</code>
*   `edges[i].length == 3`
*   <code>0 <= A<sub>i</sub>, B<sub>i</sub> < n</code>
*   <code>A<sub>i</sub> != B<sub>i</sub></code>
*   <code>1 <= W<sub>i</sub> <= 10<sup>6</sup></code>
*   There **may be** multiple edges between a pair of nodes, but they must have unique weights.

## Solution

```kotlin
import java.util.LinkedList
import java.util.Queue
import kotlin.math.max

@Suppress("unused")
class Solution {
    fun minMaxWeight(n: Int, edges: Array<IntArray>, threshold: Int): Int {
        val reversedG: Array<MutableList<IntArray>> = Array<MutableList<IntArray>>(n) { ArrayList<IntArray>() }
        for (i in edges) {
            val a = i[0]
            val b = i[1]
            val w = i[2]
            reversedG[b].add(intArrayOf(a, w))
        }
        val distance = IntArray(n)
        distance.fill(Int.Companion.MAX_VALUE)
        distance[0] = 0
        if (reversedG[0].isEmpty()) {
            return -1
        }
        val que: Queue<Int> = LinkedList<Int>()
        que.add(0)
        while (que.isNotEmpty()) {
            val cur: Int = que.poll()!!
            for (next in reversedG[cur]) {
                val node = next[0]
                val w = next[1]
                val nextdis = max(w, distance[cur])
                if (nextdis < distance[node]) {
                    distance[node] = nextdis
                    que.add(node)
                }
            }
        }
        var ans = 0
        for (i in 0..<n) {
            if (distance[i] == Int.Companion.MAX_VALUE) {
                return -1
            }
            ans = max(ans, distance[i])
        }
        return ans
    }
}
```