[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1976\. Number of Ways to Arrive at Destination

Medium

You are in a city that consists of `n` intersections numbered from `0` to `n - 1` with **bi-directional** roads between some intersections. The inputs are generated such that you can reach any intersection from any other intersection and that there is at most one road between any two intersections.

You are given an integer `n` and a 2D integer array `roads` where <code>roads[i] = [u<sub>i</sub>, v<sub>i</sub>, time<sub>i</sub>]</code> means that there is a road between intersections <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code> that takes <code>time<sub>i</sub></code> minutes to travel. You want to know in how many ways you can travel from intersection `0` to intersection `n - 1` in the **shortest amount of time**.

Return _the **number of ways** you can arrive at your destination in the **shortest amount of time**_. Since the answer may be large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/07/17/graph2.png)

**Input:** n = 7, roads = \[\[0,6,7],[0,1,2],[1,2,3],[1,3,3],[6,3,3],[3,5,1],[6,5,1],[2,5,1],[0,4,5],[4,6,2]]

**Output:** 4

**Explanation:** The shortest amount of time it takes to go from intersection 0 to intersection 6 is 7 minutes. The four ways to get there in 7 minutes are: 

- 0 ➝ 6 

- 0 ➝ 4 ➝ 6 

- 0 ➝ 1 ➝ 2 ➝ 5 ➝ 6 

- 0 ➝ 1 ➝ 3 ➝ 5 ➝ 6

**Example 2:**

**Input:** n = 2, roads = \[\[1,0,10]]

**Output:** 1

**Explanation:** There is only one way to go from intersection 0 to intersection 1, and it takes 10 minutes.

**Constraints:**

*   `1 <= n <= 200`
*   `n - 1 <= roads.length <= n * (n - 1) / 2`
*   `roads[i].length == 3`
*   <code>0 <= u<sub>i</sub>, v<sub>i</sub> <= n - 1</code>
*   <code>1 <= time<sub>i</sub> <= 10<sup>9</sup></code>
*   <code>u<sub>i</sub> != v<sub>i</sub></code>
*   There is at most one road connecting any two intersections.
*   You can reach any intersection from any other intersection.

## Solution

```kotlin
import java.util.PriorityQueue
import java.util.Queue

class Solution {
    private fun dijkstra(roads: Array<IntArray>, n: Int): Int {
        val mod = 1e9.toInt() + 7L
        val pq: Queue<LongArray> = PriorityQueue({ l1: LongArray, l2: LongArray -> l1[1].compareTo(l2[1]) })
        val ways = LongArray(n)
        val dist = LongArray(n)
        dist.fill(1e18.toLong())
        dist[0] = 0
        ways[0] = 1
        val graph: Array<ArrayList<LongArray>?> = arrayOfNulls<ArrayList<LongArray>>(n)
        for (i in graph.indices) {
            graph[i] = ArrayList()
        }
        for (road in roads) {
            graph[road[0]]?.add(longArrayOf(road[1].toLong(), road[2].toLong()))
            graph[road[1]]?.add(longArrayOf(road[0].toLong(), road[2].toLong()))
        }
        pq.add(longArrayOf(0, 0))
        if (pq.isNotEmpty()) {
            while (pq.isNotEmpty()) {
                val ele = pq.remove()
                val dis = ele[1]
                val node = ele[0]
                for (e in graph[node.toInt()]!!) {
                    val wt = e[1]
                    val adjNode = e[0]
                    if (wt + dis < dist[adjNode.toInt()]) {
                        dist[adjNode.toInt()] = wt + dis
                        ways[adjNode.toInt()] = ways[node.toInt()]
                        pq.add(longArrayOf(adjNode, dist[adjNode.toInt()]))
                    } else if (wt + dis == dist[adjNode.toInt()]) {
                        ways[adjNode.toInt()] = (ways[node.toInt()] + ways[adjNode.toInt()]) % mod
                    }
                }
            }
        }
        return ways[n - 1].toInt()
    }

    fun countPaths(n: Int, roads: Array<IntArray>): Int {
        return dijkstra(roads, n)
    }
}
```