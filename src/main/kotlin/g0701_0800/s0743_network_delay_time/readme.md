[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 743\. Network Delay Time

Medium

You are given a network of `n` nodes, labeled from `1` to `n`. You are also given `times`, a list of travel times as directed edges <code>times[i] = (u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>)</code>, where <code>u<sub>i</sub></code> is the source node, <code>v<sub>i</sub></code> is the target node, and <code>w<sub>i</sub></code> is the time it takes for a signal to travel from source to target.

We will send a signal from a given node `k`. Return _the **minimum** time it takes for all the_ `n` _nodes to receive the signal_. If it is impossible for all the `n` nodes to receive the signal, return `-1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/05/23/931_example_1.png)

**Input:** times = \[\[2,1,1],[2,3,1],[3,4,1]], n = 4, k = 2

**Output:** 2

**Example 2:**

**Input:** times = \[\[1,2,1]], n = 2, k = 1

**Output:** 1

**Example 3:**

**Input:** times = \[\[1,2,1]], n = 2, k = 2

**Output:** -1

**Constraints:**

*   `1 <= k <= n <= 100`
*   `1 <= times.length <= 6000`
*   `times[i].length == 3`
*   <code>1 <= u<sub>i</sub>, v<sub>i</sub> <= n</code>
*   <code>u<sub>i</sub> != v<sub>i</sub></code>
*   <code>0 <= w<sub>i</sub> <= 100</code>
*   All the pairs <code>(u<sub>i</sub>, v<sub>i</sub>)</code> are **unique**. (i.e., no multiple edges.)

## Solution

```kotlin
import java.util.LinkedList
import java.util.Queue

class Solution {
    fun networkDelayTime(times: Array<IntArray>, n: Int, k: Int): Int {
        val graph = Array(n + 1) { IntArray(n + 1) }
        for (g in graph) {
            g.fill(-1)
        }
        for (t in times) {
            graph[t[0]][t[1]] = t[2]
        }
        val visited = BooleanArray(n + 1)
        val dist = IntArray(n + 1)
        dist.fill(Int.MAX_VALUE)
        dist[k] = 0
        val spf: Queue<Int> = LinkedList()
        spf.add(k)
        visited[k] = true
        while (!spf.isEmpty()) {
            val curr = spf.poll()
            visited[curr] = false
            for (i in 1..n) {
                if (graph[curr][i] != -1 && dist[i] > dist[curr] + graph[curr][i]) {
                    dist[i] = dist[curr] + graph[curr][i]
                    if (!visited[i]) {
                        spf.add(i)
                        visited[i] = true
                    }
                }
            }
        }
        var result = 0
        for (i in 1..n) {
            result = dist[i].coerceAtLeast(result)
        }
        return if (result == Int.MAX_VALUE) -1 else result
    }
}
```