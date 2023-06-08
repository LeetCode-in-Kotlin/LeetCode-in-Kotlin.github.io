[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1334\. Find the City With the Smallest Number of Neighbors at a Threshold Distance

Medium

There are `n` cities numbered from `0` to `n-1`. Given the array `edges` where <code>edges[i] = [from<sub>i</sub>, to<sub>i</sub>, weight<sub>i</sub>]</code> represents a bidirectional and weighted edge between cities <code>from<sub>i</sub></code> and <code>to<sub>i</sub></code>, and given the integer `distanceThreshold`.

Return the city with the smallest number of cities that are reachable through some path and whose distance is **at most** `distanceThreshold`, If there are multiple such cities, return the city with the greatest number.

Notice that the distance of a path connecting cities _**i**_ and _**j**_ is equal to the sum of the edges' weights along that path.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/01/16/find_the_city_01.png)

**Input:** n = 4, edges = \[\[0,1,3],[1,2,1],[1,3,4],[2,3,1]], distanceThreshold = 4

**Output:** 3

**Explanation:** The figure above describes the graph. 

The neighboring cities at a distanceThreshold = 4 for each city are: 

City 0 -> [City 1, City 2] 

City 1 -> [City 0, City 2, City 3] 

City 2 -> [City 0, City 1, City 3]

City 3 -> [City 1, City 2] 

Cities 0 and 3 have 2 neighboring cities at a distanceThreshold = 4, but we have to return city 3 since it has the greatest number.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/01/16/find_the_city_02.png)

**Input:** n = 5, edges = \[\[0,1,2],[0,4,8],[1,2,3],[1,4,2],[2,3,1],[3,4,1]], distanceThreshold = 2

**Output:** 0

**Explanation:** The figure above describes the graph.

The neighboring cities at a distanceThreshold = 2 for each city are: 

City 0 -> [City 1] 

City 1 -> [City 0, City 4]

City 2 -> [City 3, City 4] 

City 3 -> [City 2, City 4]

City 4 -> [City 1, City 2, City 3] 

The city 0 has 1 neighboring city at a distanceThreshold = 2.

**Constraints:**

*   `2 <= n <= 100`
*   `1 <= edges.length <= n * (n - 1) / 2`
*   `edges[i].length == 3`
*   <code>0 <= from<sub>i</sub> < to<sub>i</sub> < n</code>
*   <code>1 <= weight<sub>i</sub>, distanceThreshold <= 10^4</code>
*   All pairs <code>(from<sub>i</sub>, to<sub>i</sub>)</code> are distinct.

## Solution

```kotlin
class Solution {
    fun findTheCity(n: Int, edges: Array<IntArray>, maxDist: Int): Int {
        val graph = Array(n) { IntArray(n) }
        for (edge in edges) {
            graph[edge[0]][edge[1]] = edge[2]
            graph[edge[1]][edge[0]] = edge[2]
        }
        return fllowdWarshall(graph, n, maxDist)
    }

    private fun fllowdWarshall(graph: Array<IntArray>, n: Int, maxDist: Int): Int {
        val inf = 10001
        val dist = Array(n) { IntArray(n) }
        for (i in 0 until n) {
            for (j in 0 until n) {
                if (i != j && graph[i][j] == 0) {
                    dist[i][j] = inf
                } else {
                    dist[i][j] = graph[i][j]
                }
            }
        }
        for (k in 0 until n) {
            for (i in 0 until n) {
                for (j in 0 until n) {
                    if (dist[i][k] + dist[k][j] < dist[i][j]) {
                        dist[i][j] = dist[i][k] + dist[k][j]
                    }
                }
            }
        }
        return getList(dist, n, maxDist)
    }

    private fun getList(dist: Array<IntArray>, n: Int, maxDist: Int): Int {
        val map = HashMap<Int, MutableList<Int>>()
        for (i in 0 until n) {
            for (j in 0 until n) {
                if (!map.containsKey(i)) {
                    map[i] = ArrayList()
                    if (dist[i][j] <= maxDist && i != j) {
                        map[i]!!.add(j)
                    }
                } else if (map.containsKey(i) && dist[i][j] <= maxDist && i != j) {
                    map[i]!!.add(j)
                }
            }
        }
        var numOfEle = Int.MAX_VALUE
        var ans = 0
        for (i in 0 until n) {
            if (numOfEle >= map[i]!!.size) {
                numOfEle = Math.min(numOfEle, map[i]!!.size)
                ans = i
            }
        }
        return ans
    }
}
```