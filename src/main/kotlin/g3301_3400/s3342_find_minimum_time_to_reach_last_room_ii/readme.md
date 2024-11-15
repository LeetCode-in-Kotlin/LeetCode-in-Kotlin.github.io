[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3342\. Find Minimum Time to Reach Last Room II

Medium

There is a dungeon with `n x m` rooms arranged as a grid.

You are given a 2D array `moveTime` of size `n x m`, where `moveTime[i][j]` represents the **minimum** time in seconds when you can **start moving** to that room. You start from the room `(0, 0)` at time `t = 0` and can move to an **adjacent** room. Moving between **adjacent** rooms takes one second for one move and two seconds for the next, **alternating** between the two.

Return the **minimum** time to reach the room `(n - 1, m - 1)`.

Two rooms are **adjacent** if they share a common wall, either _horizontally_ or _vertically_.

**Example 1:**

**Input:** moveTime = \[\[0,4],[4,4]]

**Output:** 7

**Explanation:**

The minimum time required is 7 seconds.

*   At time `t == 4`, move from room `(0, 0)` to room `(1, 0)` in one second.
*   At time `t == 5`, move from room `(1, 0)` to room `(1, 1)` in two seconds.

**Example 2:**

**Input:** moveTime = \[\[0,0,0,0],[0,0,0,0]]

**Output:** 6

**Explanation:**

The minimum time required is 6 seconds.

*   At time `t == 0`, move from room `(0, 0)` to room `(1, 0)` in one second.
*   At time `t == 1`, move from room `(1, 0)` to room `(1, 1)` in two seconds.
*   At time `t == 3`, move from room `(1, 1)` to room `(1, 2)` in one second.
*   At time `t == 4`, move from room `(1, 2)` to room `(1, 3)` in two seconds.

**Example 3:**

**Input:** moveTime = \[\[0,1],[1,2]]

**Output:** 4

**Constraints:**

*   `2 <= n == moveTime.length <= 750`
*   `2 <= m == moveTime[i].length <= 750`
*   <code>0 <= moveTime[i][j] <= 10<sup>9</sup></code>

## Solution

```kotlin
import java.util.Comparator
import java.util.PriorityQueue
import kotlin.math.max

class Solution {
    private class Node {
        var x: Int = 0
        var y: Int = 0
        var t: Int = 0
        var turn: Int = 0
    }

    private val dir = arrayOf<IntArray?>(intArrayOf(1, 0), intArrayOf(-1, 0), intArrayOf(0, 1), intArrayOf(0, -1))

    fun minTimeToReach(moveTime: Array<IntArray>): Int {
        val pq = PriorityQueue<Node>(Comparator { a: Node, b: Node -> a.t - b.t })
        val m = moveTime.size
        val n = moveTime[0].size
        val node = Node()
        node.x = 0
        node.y = 0
        var t = 0
        node.t = t
        node.turn = 0
        pq.add(node)
        moveTime[0][0] = -1
        while (pq.isNotEmpty()) {
            val curr = pq.poll()
            for (i in 0..3) {
                val x = curr.x + dir[i]!![0]
                val y = curr.y + dir[i]!![1]
                if (x == m - 1 && y == n - 1) {
                    t = max(curr.t, moveTime[x][y]) + 1 + curr.turn
                    return t
                }
                if (x >= 0 && x < m && y < n && y >= 0 && moveTime[x][y] != -1) {
                    val newNode = Node()
                    t = max(curr.t, moveTime[x][y]) + 1 + curr.turn
                    newNode.x = x
                    newNode.y = y
                    newNode.t = t
                    newNode.turn = if (curr.turn == 1) 0 else 1
                    pq.add(newNode)
                    moveTime[x][y] = -1
                }
            }
        }
        return -1
    }
}
```