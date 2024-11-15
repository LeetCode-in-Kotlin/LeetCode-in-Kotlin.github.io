[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3341\. Find Minimum Time to Reach Last Room I

Medium

There is a dungeon with `n x m` rooms arranged as a grid.

You are given a 2D array `moveTime` of size `n x m`, where `moveTime[i][j]` represents the **minimum** time in seconds when you can **start moving** to that room. You start from the room `(0, 0)` at time `t = 0` and can move to an **adjacent** room. Moving between adjacent rooms takes _exactly_ one second.

Return the **minimum** time to reach the room `(n - 1, m - 1)`.

Two rooms are **adjacent** if they share a common wall, either _horizontally_ or _vertically_.

**Example 1:**

**Input:** moveTime = \[\[0,4],[4,4]]

**Output:** 6

**Explanation:**

The minimum time required is 6 seconds.

*   At time `t == 4`, move from room `(0, 0)` to room `(1, 0)` in one second.
*   At time `t == 5`, move from room `(1, 0)` to room `(1, 1)` in one second.

**Example 2:**

**Input:** moveTime = \[\[0,0,0],[0,0,0]]

**Output:** 3

**Explanation:**

The minimum time required is 3 seconds.

*   At time `t == 0`, move from room `(0, 0)` to room `(1, 0)` in one second.
*   At time `t == 1`, move from room `(1, 0)` to room `(1, 1)` in one second.
*   At time `t == 2`, move from room `(1, 1)` to room `(1, 2)` in one second.

**Example 3:**

**Input:** moveTime = \[\[0,1],[1,2]]

**Output:** 3

**Constraints:**

*   `2 <= n == moveTime.length <= 50`
*   `2 <= m == moveTime[i].length <= 50`
*   <code>0 <= moveTime[i][j] <= 10<sup>9</sup></code>

## Solution

```kotlin
import java.util.Comparator
import java.util.PriorityQueue
import java.util.function.ToIntFunction
import kotlin.math.max

class Solution {
    fun minTimeToReach(moveTime: Array<IntArray>): Int {
        val rows = moveTime.size
        val cols = moveTime[0].size
        val minHeap =
            PriorityQueue<IntArray>(Comparator.comparingInt<IntArray>(ToIntFunction { a: IntArray -> a[0] }))
        val time: Array<IntArray> = Array<IntArray>(rows) { IntArray(cols) }
        for (row in time) {
            row.fill(Int.Companion.MAX_VALUE)
        }
        minHeap.offer(intArrayOf(0, 0, 0))
        time[0][0] = 0
        val directions = arrayOf<IntArray>(intArrayOf(1, 0), intArrayOf(-1, 0), intArrayOf(0, 1), intArrayOf(0, -1))
        while (minHeap.isNotEmpty()) {
            val current = minHeap.poll()
            val currentTime = current[0]
            val x = current[1]
            val y = current[2]
            if (x == rows - 1 && y == cols - 1) {
                return currentTime
            }
            for (dir in directions) {
                val newX = x + dir[0]
                val newY = y + dir[1]
                if (newX >= 0 && newX < rows && newY >= 0 && newY < cols) {
                    val waitTime: Int = max((moveTime[newX][newY] - currentTime), 0)
                    val newTime = currentTime + 1 + waitTime
                    if (newTime < time[newX][newY]) {
                        time[newX][newY] = newTime
                        minHeap.offer(intArrayOf(newTime, newX, newY))
                    }
                }
            }
        }
        return -1
    }
}
```