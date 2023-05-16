[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 994\. Rotting Oranges

Medium

You are given an `m x n` `grid` where each cell can have one of three values:

*   `0` representing an empty cell,
*   `1` representing a fresh orange, or
*   `2` representing a rotten orange.

Every minute, any fresh orange that is **4-directionally adjacent** to a rotten orange becomes rotten.

Return _the minimum number of minutes that must elapse until no cell has a fresh orange_. If _this is impossible, return_ `-1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/02/16/oranges.png)

**Input:** grid = \[\[2,1,1],[1,1,0],[0,1,1]]

**Output:** 4

**Example 2:**

**Input:** grid = \[\[2,1,1],[0,1,1],[1,0,1]]

**Output:** -1

**Explanation:** The orange in the bottom left corner (row 2, column 0) is never rotten, because rotting only happens 4-directionally.

**Example 3:**

**Input:** grid = \[\[0,2]]

**Output:** 0

**Explanation:** Since there are already no fresh oranges at minute 0, the answer is just 0.

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   `1 <= m, n <= 10`
*   `grid[i][j]` is `0`, `1`, or `2`.

## Solution

```kotlin
import java.util.LinkedList
import java.util.Queue

class Solution {
    fun orangesRotting(grid: Array<IntArray>): Int {
        val queue: Queue<IntArray> = LinkedList()
        val row = grid.size
        val col: Int = grid[0].size
        var countActive = 0
        for (i in 0 until row) {
            for (j in 0 until col) {
                if (grid[i][j] == 2) {
                    queue.add(intArrayOf(i, j))
                }
                if (grid[i][j] != 0) {
                    countActive++
                }
            }
        }
        if (countActive == 0) {
            return 0
        }
        var countCurrent = 0
        var count = 0
        val dx = intArrayOf(0, 0, 1, -1)
        val dy = intArrayOf(1, -1, 0, 0)
        while (queue.isNotEmpty()) {
            val size: Int = queue.size
            count += size
            for (i in 0 until size) {
                val arr: IntArray = queue.poll()
                for (j in 0..3) {
                    val x = arr[0] + dx[j]
                    val y = arr[1] + dy[j]
                    if (x < 0 || y < 0 || x >= row || y >= col || grid[x][y] != 1) {
                        continue
                    }
                    grid[x][y] = 2
                    queue.add(intArrayOf(x, y))
                }
            }
            if (queue.isNotEmpty()) {
                countCurrent++
            }
        }
        return if (countActive == count) countCurrent else -1
    }
}
```