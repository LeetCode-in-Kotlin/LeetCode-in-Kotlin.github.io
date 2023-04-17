[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 815\. Bus Routes

Hard

You are given an array `routes` representing bus routes where `routes[i]` is a bus route that the <code>i<sup>th</sup></code> bus repeats forever.

*   For example, if `routes[0] = [1, 5, 7]`, this means that the <code>0<sup>th</sup></code> bus travels in the sequence `1 -> 5 -> 7 -> 1 -> 5 -> 7 -> 1 -> ...` forever.

You will start at the bus stop `source` (You are not on any bus initially), and you want to go to the bus stop `target`. You can travel between bus stops by buses only.

Return _the least number of buses you must take to travel from_ `source` _to_ `target`. Return `-1` if it is not possible.

**Example 1:**

**Input:** routes = \[\[1,2,7],[3,6,7]], source = 1, target = 6

**Output:** 2

**Explanation:** The best strategy is take the first bus to the bus stop 7, then take the second bus to the bus stop 6.

**Example 2:**

**Input:** routes = \[\[7,12],[4,5,15],[6],[15,19],[9,12,13]], source = 15, target = 12

**Output:** -1

**Constraints:**

*   `1 <= routes.length <= 500`.
*   <code>1 <= routes[i].length <= 10<sup>5</sup></code>
*   All the values of `routes[i]` are **unique**.
*   <code>sum(routes[i].length) <= 10<sup>5</sup></code>
*   <code>0 <= routes[i][j] < 10<sup>6</sup></code>
*   <code>0 <= source, target < 10<sup>6</sup></code>

## Solution

```kotlin
import java.util.LinkedList
import java.util.Queue

class Solution {
    fun numBusesToDestination(routes: Array<IntArray>, source: Int, target: Int): Int {
        if (source == target) {
            return 0
        }
        val targetRoutes: MutableSet<Int> = HashSet()
        val queue: Queue<Int> = LinkedList()
        val taken = BooleanArray(routes.size)
        val graph = buildGraph(routes, source, target, queue, targetRoutes, taken)
        if (targetRoutes.isEmpty()) {
            return -1
        }
        var bus = 1
        while (!queue.isEmpty()) {
            val size = queue.size
            for (i in 0 until size) {
                val route = queue.poll()
                if (targetRoutes.contains(route)) {
                    return bus
                }
                for (nextRoute in graph[route]!!) {
                    if (!taken[nextRoute]) {
                        queue.offer(nextRoute)
                        taken[nextRoute] = true
                    }
                }
            }
            bus++
        }
        return -1
    }

    private fun buildGraph(
        routes: Array<IntArray>,
        source: Int,
        target: Int,
        queue: Queue<Int>,
        targetRoutes: MutableSet<Int>,
        taken: BooleanArray
    ): Array<ArrayList<Int>?> {
        val len = routes.size
        val graph: Array<ArrayList<Int>?> = arrayOfNulls(len)
        for (i in 0 until len) {
            routes[i].sort()
            graph[i] = ArrayList()
            var id = routes[i].binarySearch(source)
            if (id >= 0) {
                queue.offer(i)
                taken[i] = true
            }
            id = routes[i].binarySearch(target)
            if (id >= 0) {
                targetRoutes.add(i)
            }
        }
        for (i in 0 until len) {
            for (j in i + 1 until len) {
                if (commonStop(routes[i], routes[j])) {
                    graph[i]?.add(j)
                    graph[j]?.add(i)
                }
            }
        }
        return graph
    }

    private fun commonStop(routeA: IntArray, routeB: IntArray): Boolean {
        var idA = 0
        var idB = 0
        while (idA < routeA.size && idB < routeB.size) {
            if (routeA[idA] == routeB[idB]) {
                return true
            } else if (routeA[idA] < routeB[idB]) {
                idA++
            } else {
                idB++
            }
        }
        return false
    }
}
```