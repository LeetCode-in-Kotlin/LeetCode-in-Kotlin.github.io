[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1391\. Check if There is a Valid Path in a Grid

Medium

You are given an `m x n` `grid`. Each cell of `grid` represents a street. The street of `grid[i][j]` can be:

*   `1` which means a street connecting the left cell and the right cell.
*   `2` which means a street connecting the upper cell and the lower cell.
*   `3` which means a street connecting the left cell and the lower cell.
*   `4` which means a street connecting the right cell and the lower cell.
*   `5` which means a street connecting the left cell and the upper cell.
*   `6` which means a street connecting the right cell and the upper cell.

![](https://assets.leetcode.com/uploads/2020/03/05/main.png)

You will initially start at the street of the upper-left cell `(0, 0)`. A valid path in the grid is a path that starts from the upper left cell `(0, 0)` and ends at the bottom-right cell `(m - 1, n - 1)`. **The path should only follow the streets**.

**Notice** that you are **not allowed** to change any street.

Return `true` _if there is a valid path in the grid or_ `false` _otherwise_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/03/05/e1.png)

**Input:** grid = \[\[2,4,3],[6,5,2]]

**Output:** true

**Explanation:** As shown you can start at cell (0, 0) and visit all the cells of the grid to reach (m - 1, n - 1).

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/03/05/e2.png)

**Input:** grid = \[\[1,2,1],[1,2,1]]

**Output:** false

**Explanation:** As shown you the street at cell (0, 0) is not connected with any street of any other cell and you will get stuck at cell (0, 0)

**Example 3:**

**Input:** grid = \[\[1,1,2]]

**Output:** false

**Explanation:** You will get stuck at cell (0, 1) and you cannot reach cell (0, 2).

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   `1 <= m, n <= 300`
*   `1 <= grid[i][j] <= 6`

## Solution

```kotlin
import java.util.LinkedList
import java.util.Queue

class Solution {
    private val dirs = arrayOf(
        arrayOf(intArrayOf(0, -1), intArrayOf(0, 1)),
        arrayOf(intArrayOf(-1, 0), intArrayOf(1, 0)),
        arrayOf(
            intArrayOf(0, -1),
            intArrayOf(1, 0)
        ),
        arrayOf(intArrayOf(0, 1), intArrayOf(1, 0)),
        arrayOf(intArrayOf(0, -1), intArrayOf(-1, 0)),
        arrayOf(
            intArrayOf(0, 1),
            intArrayOf(-1, 0)
        )
    )

    // the idea is you need to check port direction match, you can go to next cell and check whether
    // you can come back.
    fun hasValidPath(grid: Array<IntArray>): Boolean {
        val m = grid.size
        val n = grid[0].size
        val visited = Array(m) { BooleanArray(n) }
        val q: Queue<IntArray> = LinkedList()
        q.add(intArrayOf(0, 0))
        visited[0][0] = true
        while (q.isNotEmpty()) {
            val cur = q.poll()
            val x = cur[0]
            val y = cur[1]
            val num = grid[x][y] - 1
            for (dir in dirs[num]) {
                val nx = x + dir[0]
                val ny = y + dir[1]
                if (nx < 0 || nx >= m || ny < 0 || ny >= n || visited[nx][ny]) {
                    continue
                }
                // go to the next cell and come back to orign to see if port directions are same
                for (backDir in dirs[grid[nx][ny] - 1]) {
                    if (nx + backDir[0] == x && ny + backDir[1] == y) {
                        visited[nx][ny] = true
                        q.add(intArrayOf(nx, ny))
                    }
                }
            }
        }
        return visited[m - 1][n - 1]
    }
}
```