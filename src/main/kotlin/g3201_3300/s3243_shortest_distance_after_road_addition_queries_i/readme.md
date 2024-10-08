[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3243\. Shortest Distance After Road Addition Queries I

Medium

You are given an integer `n` and a 2D integer array `queries`.

There are `n` cities numbered from `0` to `n - 1`. Initially, there is a **unidirectional** road from city `i` to city `i + 1` for all `0 <= i < n - 1`.

<code>queries[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> represents the addition of a new **unidirectional** road from city <code>u<sub>i</sub></code> to city <code>v<sub>i</sub></code>. After each query, you need to find the **length** of the **shortest path** from city `0` to city `n - 1`.

Return an array `answer` where for each `i` in the range `[0, queries.length - 1]`, `answer[i]` is the _length of the shortest path_ from city `0` to city `n - 1` after processing the **first** `i + 1` queries.

**Example 1:**

**Input:** n = 5, queries = \[\[2,4],[0,2],[0,4]]

**Output:** [3,2,1]

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/06/28/image8.jpg)

After the addition of the road from 2 to 4, the length of the shortest path from 0 to 4 is 3.

![](https://assets.leetcode.com/uploads/2024/06/28/image9.jpg)

After the addition of the road from 0 to 2, the length of the shortest path from 0 to 4 is 2.

![](https://assets.leetcode.com/uploads/2024/06/28/image10.jpg)

After the addition of the road from 0 to 4, the length of the shortest path from 0 to 4 is 1.

**Example 2:**

**Input:** n = 4, queries = \[\[0,3],[0,2]]

**Output:** [1,1]

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/06/28/image11.jpg)

After the addition of the road from 0 to 3, the length of the shortest path from 0 to 3 is 1.

![](https://assets.leetcode.com/uploads/2024/06/28/image12.jpg)

After the addition of the road from 0 to 2, the length of the shortest path remains 1.

**Constraints:**

*   `3 <= n <= 500`
*   `1 <= queries.length <= 500`
*   `queries[i].length == 2`
*   `0 <= queries[i][0] < queries[i][1] < n`
*   `1 < queries[i][1] - queries[i][0]`
*   There are no repeated roads among the queries.

## Solution

```kotlin
class Solution {
    fun shortestDistanceAfterQueries(n: Int, queries: Array<IntArray>): IntArray {
        val dist = IntArray(n)
        for (i in 0 until n) {
            dist[i] = i
        }
        val parent: Array<ArrayList<Int>> = Array(n) { ArrayList() }
        for (i in 0 until n) {
            parent[i] = ArrayList()
            if (i != n - 1) {
                parent[i].add(i + 1)
            }
        }
        val ans = IntArray(queries.size)
        for (i in queries.indices) {
            val u = queries[i][0]
            val v = queries[i][1]
            if (dist[v] > dist[u] + 1) {
                dist[v] = dist[u] + 1
                parent[u].add(v)
                updateDistance(v, dist, parent)
            } else {
                parent[u].add(v)
            }
            ans[i] = dist[n - 1]
        }
        return ans
    }

    fun updateDistance(par: Int, dist: IntArray, parent: Array<ArrayList<Int>>) {
        for (i in parent[par].indices) {
            val child = parent[par][i]
            if (dist[child] > dist[par] + 1) {
                dist[child] = dist[par] + 1
                updateDistance(child, dist, parent)
            }
        }
    }
}
```