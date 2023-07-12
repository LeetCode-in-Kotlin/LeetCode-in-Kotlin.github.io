[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2577\. Minimum Time to Visit a Cell In a Grid

Hard

You are given a `m x n` matrix `grid` consisting of **non-negative** integers where `grid[row][col]` represents the **minimum** time required to be able to visit the cell `(row, col)`, which means you can visit the cell `(row, col)` only when the time you visit it is greater than or equal to `grid[row][col]`.

You are standing in the **top-left** cell of the matrix in the <code>0<sup>th</sup></code> second, and you must move to **any** adjacent cell in the four directions: up, down, left, and right. Each move you make takes 1 second.

Return _the **minimum** time required in which you can visit the bottom-right cell of the matrix_. If you cannot visit the bottom-right cell, then return `-1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2023/02/14/yetgriddrawio-8.png)

**Input:** grid = \[\[0,1,3,2],[5,1,2,5],[4,3,8,6]]

**Output:** 7

**Explanation:** One of the paths that we can take is the following:
- at t = 0, we are on the cell (0,0). 
- at t = 1, we move to the cell (0,1). It is possible because grid[0][1] <= 1. 
- at t = 2, we move to the cell (1,1). It is possible because grid[1][1] <= 2. 
- at t = 3, we move to the cell (1,2). It is possible because grid[1][2] <= 3. 
- at t = 4, we move to the cell (1,1). It is possible because grid[1][1] <= 4. 
- at t = 5, we move to the cell (1,2). It is possible because grid[1][2] <= 5. 
- at t = 6, we move to the cell (1,3). It is possible because grid[1][3] <= 6. 
- at t = 7, we move to the cell (2,3). It is possible because grid[2][3] <= 7. 

The final time is 7. It can be shown that it is the minimum time possible.

**Example 2:**

![](https://assets.leetcode.com/uploads/2023/02/14/yetgriddrawio-9.png)

**Input:** grid = \[\[0,2,4],[3,2,1],[1,0,4]]

**Output:** -1

**Explanation:** There is no path from the top left to the bottom-right cell.

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   `2 <= m, n <= 1000`
*   <code>4 <= m * n <= 10<sup>5</sup></code>
*   <code>0 <= grid[i][j] <= 10<sup>5</sup></code>
*   `grid[0][0] == 0`

## Solution

```kotlin
import java.util.PriorityQueue

class Solution {
    fun minimumTime(grid: Array<IntArray>): Int {
        if (grid[0][1] <= 1 || grid[1][0] <= 1) {
            val m = grid.size
            val n = grid[0].size
            val pq = PriorityQueue { a: IntArray, b: IntArray ->
                a[0] - b[0]
            }
            val dist: MutableMap<String, Int> = HashMap()
            pq.offer(intArrayOf(0, 0, 0))
            dist.put("0,0", 0)
            while (pq.isNotEmpty()) {
                val curr = pq.poll()
                val x = curr[0]
                val i = curr[1]
                val j = curr[2]
                if (i == m - 1 && j == n - 1) {
                    return x
                }
                val directions =
                    arrayOf(intArrayOf(i - 1, j), intArrayOf(i, j - 1), intArrayOf(i, j + 1), intArrayOf(i + 1, j))
                for (dir in directions) {
                    val ii = dir[0]
                    val jj = dir[1]
                    if (ii in 0 until m && jj >= 0 && jj < n) {
                        val xx = x + 1 + 0.coerceAtLeast((grid[ii][jj] - x) / 2 * 2)
                        val key = "$ii,$jj"
                        if (!dist.containsKey(key) || dist[key]!! > xx) {
                            pq.offer(intArrayOf(xx, ii, jj))
                            dist.put(key, xx)
                        }
                    }
                }
            }
        }
        return -1
    }
}
```