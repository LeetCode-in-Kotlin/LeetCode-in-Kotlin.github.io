[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3620\. Network Recovery Pathways

Hard

You are given a directed acyclic graph of `n` nodes numbered from 0 to `n − 1`. This is represented by a 2D array `edges` of length `m`, where <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, cost<sub>i</sub>]</code> indicates a one‑way communication from node <code>u<sub>i</sub></code> to node <code>v<sub>i</sub></code> with a recovery cost of <code>cost<sub>i</sub></code>.

Some nodes may be offline. You are given a boolean array `online` where `online[i] = true` means node `i` is online. Nodes 0 and `n − 1` are always online.

A path from 0 to `n − 1` is **valid** if:

*   All intermediate nodes on the path are online.
*   The total recovery cost of all edges on the path does not exceed `k`.

For each valid path, define its **score** as the minimum edge‑cost along that path.

Return the **maximum** path score (i.e., the largest **minimum**\-edge cost) among all valid paths. If no valid path exists, return -1.

**Example 1:**

**Input:** edges = \[\[0,1,5],[1,3,10],[0,2,3],[2,3,4]], online = [true,true,true,true], k = 10

**Output:** 3

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/06/06/graph-10.png)

*   The graph has two possible routes from node 0 to node 3:
    
    1.  Path `0 → 1 → 3`
        
        *   Total cost = `5 + 10 = 15`, which exceeds k (`15 > 10`), so this path is invalid.
            
    2.  Path `0 → 2 → 3`
        
        *   Total cost = `3 + 4 = 7 <= k`, so this path is valid.
            
        *   The minimum edge‐cost along this path is `min(3, 4) = 3`.
            
*   There are no other valid paths. Hence, the maximum among all valid path‐scores is 3.
    

**Example 2:**

**Input:** edges = \[\[0,1,7],[1,4,5],[0,2,6],[2,3,6],[3,4,2],[2,4,6]], online = [true,true,true,false,true], k = 12

**Output:** 6

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/06/06/graph-11.png)

*   Node 3 is offline, so any path passing through 3 is invalid.
    
*   Consider the remaining routes from 0 to 4:
    
    1.  Path `0 → 1 → 4`
        
        *   Total cost = `7 + 5 = 12 <= k`, so this path is valid.
            
        *   The minimum edge‐cost along this path is `min(7, 5) = 5`.
            
    2.  Path `0 → 2 → 3 → 4`
        
        *   Node 3 is offline, so this path is invalid regardless of cost.
            
    3.  Path `0 → 2 → 4`
        
        *   Total cost = `6 + 6 = 12 <= k`, so this path is valid.
            
        *   The minimum edge‐cost along this path is `min(6, 6) = 6`.
            
*   Among the two valid paths, their scores are 5 and 6. Therefore, the answer is 6.
    

**Constraints:**

*   `n == online.length`
*   <code>2 <= n <= 5 * 10<sup>4</sup></code>
*   `0 <= m == edges.length <=` <code>min(10<sup>5</sup>, n * (n - 1) / 2)</code>
*   <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, cost<sub>i</sub>]</code>
*   <code>0 <= u<sub>i</sub>, v<sub>i</sub> < n</code>
*   <code>u<sub>i</sub> != v<sub>i</sub></code>
*   <code>0 <= cost<sub>i</sub> <= 10<sup>9</sup></code>
*   <code>0 <= k <= 5 * 10<sup>13</sup></code>
*   `online[i]` is either `true` or `false`, and both `online[0]` and `online[n − 1]` are `true`.
*   The given graph is a directed acyclic graph.

## Solution

```kotlin
import java.util.LinkedList
import java.util.Queue
import kotlin.math.max

class Solution {
    private fun topologicalSort(n: Int, g: Array<ArrayList<Int>>): List<Int> {
        val indeg = IntArray(n)
        for (i in 0 until n) {
            for (adjNode in g[i]) {
                indeg[adjNode]++
            }
        }
        val q: Queue<Int> = LinkedList()
        val ts = ArrayList<Int>()
        for (i in 0 until n) {
            if (indeg[i] == 0) {
                q.offer(i)
            }
        }
        while (q.isNotEmpty()) {
            val u = q.poll()
            ts.add(u)
            for (v in g[u]) {
                indeg[v]--
                if (indeg[v] == 0) {
                    q.offer(v)
                }
            }
        }
        return ts
    }

    private fun check(
        x: Int,
        n: Int,
        adj: Array<ArrayList<IntArray>>,
        ts: List<Int>,
        online: BooleanArray,
        k: Long,
    ): Boolean {
        val d = LongArray(n)
        d.fill(Long.MAX_VALUE)
        d[0] = 0
        for (u in ts) {
            if (d[u] != Long.MAX_VALUE) {
                for (p in adj[u]) {
                    val v = p[0]
                    val c = p[1]
                    if (c < x || !online[v]) {
                        continue
                    }
                    if (d[u] + c < d[v]) {
                        d[v] = d[u] + c
                    }
                }
            }
        }
        return d[n - 1] <= k
    }

    fun findMaxPathScore(edges: Array<IntArray>, online: BooleanArray, k: Long): Int {
        val n = online.size
        // Adjacency list for graph with edge weights
        val adj = Array<ArrayList<IntArray>>(n) { ArrayList() }
        val g = Array<ArrayList<Int>>(n) { ArrayList() }
        for (e in edges) {
            val u = e[0]
            val v = e[1]
            val c = e[2]
            adj[u].add(intArrayOf(v, c))
            g[u].add(v)
        }
        val ts = topologicalSort(n, g)
        if (!check(0, n, adj, ts, online, k)) {
            return -1
        }
        var l = 0
        var h = 0
        for (e in edges) {
            h = max(h, e[2])
        }
        while (l < h) {
            val md = l + (h - l + 1) / 2
            if (check(md, n, adj, ts, online, k)) {
                l = md
            } else {
                h = md - 1
            }
        }
        return l
    }
}
```