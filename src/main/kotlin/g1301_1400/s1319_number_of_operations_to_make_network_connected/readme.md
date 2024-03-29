[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1319\. Number of Operations to Make Network Connected

Medium

There are `n` computers numbered from `0` to `n - 1` connected by ethernet cables `connections` forming a network where <code>connections[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> represents a connection between computers <code>a<sub>i</sub></code> and <code>b<sub>i</sub></code>. Any computer can reach any other computer directly or indirectly through the network.

You are given an initial computer network `connections`. You can extract certain cables between two directly connected computers, and place them between any pair of disconnected computers to make them directly connected.

Return _the minimum number of times you need to do this in order to make all the computers connected_. If it is not possible, return `-1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/01/02/sample_1_1677.png)

**Input:** n = 4, connections = \[\[0,1],[0,2],[1,2]]

**Output:** 1

**Explanation:** Remove cable between computer 1 and 2 and place between computers 1 and 3.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/01/02/sample_2_1677.png)

**Input:** n = 6, connections = \[\[0,1],[0,2],[0,3],[1,2],[1,3]]

**Output:** 2

**Example 3:**

**Input:** n = 6, connections = \[\[0,1],[0,2],[0,3],[1,2]]

**Output:** -1

**Explanation:** There are not enough cables.

**Constraints:**

*   <code>1 <= n <= 10<sup>5</sup></code>
*   <code>1 <= connections.length <= min(n * (n - 1) / 2, 10<sup>5</sup>)</code>
*   `connections[i].length == 2`
*   <code>0 <= a<sub>i</sub>, b<sub>i</sub> < n</code>
*   <code>a<sub>i</sub> != b<sub>i</sub></code>
*   There are no repeated connections.
*   No two computers are connected by more than one cable.

## Solution

```kotlin
class Solution {
    private var disconnectedComputers = 0
    private lateinit var parent: IntArray
    private lateinit var rank: IntArray

    fun makeConnected(totalNumberOfComputers: Int, connections: Array<IntArray>): Int {
        if (connections.size < totalNumberOfComputers - 1) {
            return IMPOSSIBLE_TO_CONNECT
        }
        disconnectedComputers = totalNumberOfComputers
        rank = IntArray(totalNumberOfComputers)
        parent = IntArray(totalNumberOfComputers) { it }
        for (connection in connections) {
            unionFind(connection[0], connection[1])
        }
        return disconnectedComputers - 1
    }

    private fun unionFind(first: Int, second: Int) {
        val parentFirst = findParent(first)
        val parentSecond = findParent(second)
        if (parentFirst != parentSecond) {
            joinByRank(parentFirst, parentSecond)
            disconnectedComputers--
        }
    }

    private fun findParent(index: Int): Int {
        if (parent[index] != index) {
            parent[index] = findParent(parent[index])
        }
        return parent[index]
    }

    private fun joinByRank(first: Int, second: Int) {
        if (rank[first] < rank[second]) {
            parent[first] = second
        } else if (rank[second] < rank[first]) {
            parent[second] = first
        } else {
            parent[first] = second
            rank[second]++
        }
    }

    companion object {
        private const val IMPOSSIBLE_TO_CONNECT = -1
    }
}
```