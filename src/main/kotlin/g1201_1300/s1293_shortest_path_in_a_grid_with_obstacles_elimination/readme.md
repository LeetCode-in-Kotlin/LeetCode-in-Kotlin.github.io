[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1293\. Shortest Path in a Grid with Obstacles Elimination

Hard

You are given an `m x n` integer matrix `grid` where each cell is either `0` (empty) or `1` (obstacle). You can move up, down, left, or right from and to an empty cell in **one step**.

Return _the minimum number of **steps** to walk from the upper left corner_ `(0, 0)` _to the lower right corner_ `(m - 1, n - 1)` _given that you can eliminate **at most**_ `k` _obstacles_. If it is not possible to find such walk return `-1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/09/30/short1-grid.jpg)

**Input:** grid = \[\[0,0,0],[1,1,0],[0,0,0],[0,1,1],[0,0,0]], k = 1

**Output:** 6

**Explanation:**

The shortest path without eliminating any obstacle is 10.

The shortest path with one obstacle elimination at position (3,2) is 6. Such path is (0,0) -> (0,1) -> (0,2) -> (1,2) -> (2,2) -> **(3,2)** -> (4,2).

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/09/30/short2-grid.jpg)

**Input:** grid = \[\[0,1,1],[1,1,1],[1,0,0]], k = 1

**Output:** -1

**Explanation:** We need to eliminate at least two obstacles to find such a walk.

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   `1 <= m, n <= 40`
*   `1 <= k <= m * n`
*   `grid[i][j]` is either `0` **or** `1`.
*   `grid[0][0] == grid[m - 1][n - 1] == 0`

## Solution

```kotlin
import java.util.LinkedList
import java.util.Queue

class Solution {
    fun shortestPath(grid: Array<IntArray>, k: Int): Int {
        if (grid.size == 1 && grid[0].size == 1 && grid[0][0] == 0) {
            return 0
        }
        // 4 potential moves:
        val moves = arrayOf(intArrayOf(1, 0), intArrayOf(-1, 0), intArrayOf(0, 1), intArrayOf(0, -1))
        val m = grid.size
        val n = grid[0].size
        // use obs to record the min total obstacles when traverse to the position
        val obs = Array(m) { IntArray(n) }
        for (i in 0 until m) {
            for (j in 0 until n) {
                obs[i][j] = Int.MAX_VALUE
            }
        }
        obs[0][0] = 0
        // Queue to record {x cord, y cord, total obstacles when trvavers to this position}
        val que: Queue<IntArray> = LinkedList()
        que.add(intArrayOf(0, 0, 0))
        var level = 0
        while (que.isNotEmpty()) {
            val size = que.size
            level++
            for (i in 0 until size) {
                val current = que.poll()
                for (move in moves) {
                    val next = intArrayOf(current[0] + move[0], current[1] + move[1])
                    if (next[0] == m - 1 && next[1] == n - 1) {
                        return level
                    }
                    if (next[0] < 0 || next[0] > m - 1 || next[1] < 0 || next[1] > n - 1) {
                        continue
                    }
                    if (current[2] + grid[next[0]][next[1]] < obs[next[0]][next[1]] &&
                        current[2] + grid[next[0]][next[1]] <= k
                    ) {
                        obs[next[0]][next[1]] = current[2] + grid[next[0]][next[1]]
                        que.add(intArrayOf(next[0], next[1], obs[next[0]][next[1]]))
                    }
                }
            }
        }
        return -1
    }
}
```