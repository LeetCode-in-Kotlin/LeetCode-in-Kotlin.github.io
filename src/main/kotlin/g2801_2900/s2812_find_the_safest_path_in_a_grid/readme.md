[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2812\. Find the Safest Path in a Grid

Medium

You are given a **0-indexed** 2D matrix `grid` of size `n x n`, where `(r, c)` represents:

*   A cell containing a thief if `grid[r][c] = 1`
*   An empty cell if `grid[r][c] = 0`

You are initially positioned at cell `(0, 0)`. In one move, you can move to any adjacent cell in the grid, including cells containing thieves.

The **safeness factor** of a path on the grid is defined as the **minimum** manhattan distance from any cell in the path to any thief in the grid.

Return _the **maximum safeness factor** of all paths leading to cell_ `(n - 1, n - 1)`_._

An **adjacent** cell of cell `(r, c)`, is one of the cells `(r, c + 1)`, `(r, c - 1)`, `(r + 1, c)` and `(r - 1, c)` if it exists.

The **Manhattan distance** between two cells `(a, b)` and `(x, y)` is equal to `|a - x| + |b - y|`, where `|val|` denotes the absolute value of val.

**Example 1:**

![](https://assets.leetcode.com/uploads/2023/07/02/example1.png)

**Input:** grid = \[\[1,0,0],[0,0,0],[0,0,1]]

**Output:** 0

**Explanation:** All paths from (0, 0) to (n - 1, n - 1) go through the thieves in cells (0, 0) and (n - 1, n - 1). 

**Example 2:**

![](https://assets.leetcode.com/uploads/2023/07/02/example2.png)

**Input:** grid = \[\[0,0,1],[0,0,0],[0,0,0]]

**Output:** 2

**Explanation:**

The path depicted in the picture above has a safeness factor of 2 since:

- The closest cell of the path to the thief at cell (0, 2) is cell (0, 0).

The distance between them is \| 0 - 0 \| + \| 0 - 2 \| = 2.

It can be shown that there are no other paths with a higher safeness factor. 

**Example 3:**

![](https://assets.leetcode.com/uploads/2023/07/02/example3.png)

**Input:** grid = \[\[0,0,0,1],[0,0,0,0],[0,0,0,0],[1,0,0,0]]

**Output:** 2

**Explanation:**

The path depicted in the picture above has a safeness factor of 2 since:

- The closest cell of the path to the thief at cell (0, 3) is cell (1, 2).

The distance between them is \| 0 - 1 \| + \| 3 - 2 \| = 2.

- The closest cell of the path to the thief at cell (3, 0) is cell (3, 2).

The distance between them is \| 3 - 3 \| + \| 0 - 2 \| = 2.

It can be shown that there are no other paths with a higher safeness factor. 

**Constraints:**

*   `1 <= grid.length == n <= 400`
*   `grid[i].length == n`
*   `grid[i][j]` is either `0` or `1`.
*   There is at least one thief in the `grid`.

## Solution

```kotlin
import java.util.LinkedList
import java.util.Queue
import kotlin.math.min

class Solution {
    fun maximumSafenessFactor(grid: List<List<Int>>): Int {
        val n = grid.size
        if (grid[0][0] == 1 || grid[n - 1][n - 1] == 1) return 0
        val cost = Array(n) { IntArray(n) }
        for (v in cost) v.fill(Int.MAX_VALUE)
        bfs(cost, grid, n)
        var l = 1
        var r = n * n
        var ans = 0
        while (l <= r) {
            val mid = (r - l) / 2 + l
            if (possible(0, 0, cost, mid, n, Array(n) { BooleanArray(n) })) {
                ans = mid
                l = mid + 1
            } else {
                r = mid - 1
            }
        }
        return ans
    }

    private fun possible(
        i: Int,
        j: Int,
        cost: Array<IntArray>,
        mid: Int,
        n: Int,
        visited: Array<BooleanArray>,
    ): Boolean {
        if (i < 0 || j < 0 || i >= n || j >= n) return false
        if (cost[i][j] == Int.MAX_VALUE || cost[i][j] < mid) return false
        if (i == n - 1 && j == n - 1) return true
        if (visited[i][j]) return false
        visited[i][j] = true
        val dir = arrayOf(intArrayOf(1, 0), intArrayOf(0, 1), intArrayOf(-1, 0), intArrayOf(0, -1))
        var ans = false
        for (v in dir) {
            val ii = i + v[0]
            val jj = j + v[1]
            ans = ans or possible(ii, jj, cost, mid, n, visited)
            if (ans) return true
        }
        return ans
    }

    private fun bfs(cost: Array<IntArray>, grid: List<List<Int>>, n: Int) {
        val q: Queue<IntArray> = LinkedList()
        val visited = Array(n) { BooleanArray(n) }
        for (i in grid.indices) {
            for (j in grid.indices) {
                if (grid[i][j] == 1) {
                    q.add(intArrayOf(i, j))
                    visited[i][j] = true
                }
            }
        }
        var level = 1
        val dir = arrayOf(intArrayOf(1, 0), intArrayOf(0, 1), intArrayOf(-1, 0), intArrayOf(0, -1))
        while (q.isNotEmpty()) {
            val len = q.size
            for (i in 0 until len) {
                val v = q.poll()
                for (`val` in dir) {
                    val ii = v[0] + `val`[0]
                    val jj = v[1] + `val`[1]
                    if (isValid(ii, jj, n) && !visited[ii][jj]) {
                        q.add(intArrayOf(ii, jj))
                        cost[ii][jj] = min(cost[ii][jj].toDouble(), level.toDouble()).toInt()
                        visited[ii][jj] = true
                    }
                }
            }
            level++
        }
    }

    private fun isValid(i: Int, j: Int, n: Int): Boolean {
        return i >= 0 && j >= 0 && i < n && j < n
    }
}
```