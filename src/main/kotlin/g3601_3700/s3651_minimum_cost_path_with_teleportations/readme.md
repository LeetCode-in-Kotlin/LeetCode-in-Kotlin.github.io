[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3651\. Minimum Cost Path with Teleportations

Hard

You are given a `m x n` 2D integer array `grid` and an integer `k`. You start at the top-left cell `(0, 0)` and your goal is to reach the bottom‐right cell `(m - 1, n - 1)`.

There are two types of moves available:

*   **Normal move**: You can move right or down from your current cell `(i, j)`, i.e. you can move to `(i, j + 1)` (right) or `(i + 1, j)` (down). The cost is the value of the destination cell.
    
*   **Teleportation**: You can teleport from any cell `(i, j)`, to any cell `(x, y)` such that `grid[x][y] <= grid[i][j]`; the cost of this move is 0. You may teleport at most `k` times.
    

Return the **minimum** total cost to reach cell `(m - 1, n - 1)` from `(0, 0)`.

**Example 1:**

**Input:** grid = \[\[1,3,3],[2,5,4],[4,3,5]], k = 2

**Output:** 7

**Explanation:**

Initially we are at (0, 0) and cost is 0.

| Current Position | Move                     | New Position | Total Cost   |
|------------------|--------------------------|--------------|--------------|
| `(0, 0)`         | Move Down                | `(1, 0)`     | `0 + 2 = 2`  |
| `(1, 0)`         | Move Right               | `(1, 1)`     | `2 + 5 = 7`  |
| `(1, 1)`         | Teleport to `(2, 2)`     | `(2, 2)`     | `7 + 0 = 7`  |

The minimum cost to reach bottom-right cell is 7.

**Example 2:**

**Input:** grid = \[\[1,2],[2,3],[3,4]], k = 1

**Output:** 9

**Explanation:**

Initially we are at (0, 0) and cost is 0.

| Current Position | Move        | New Position | Total Cost   |
|------------------|-------------|--------------|--------------|
| `(0, 0)`         | Move Down   | `(1, 0)`     | `0 + 2 = 2`  |
| `(1, 0)`         | Move Right  | `(1, 1)`     | `2 + 3 = 5`  |
| `(1, 1)`         | Move Down   | `(2, 1)`     | `5 + 4 = 9`  |

The minimum cost to reach bottom-right cell is 9.

**Constraints:**

*   `2 <= m, n <= 80`
*   `m == grid.length`
*   `n == grid[i].length`
*   <code>0 <= grid[i][j] <= 10<sup>4</sup></code>
*   `0 <= k <= 10`

## Solution

```kotlin
import kotlin.math.max
import kotlin.math.min

class Solution {
    fun minCost(grid: Array<IntArray>, k: Int): Int {
        val n = grid.size
        val m = grid[0].size
        var max = -1
        val dp = Array<IntArray>(n) { IntArray(m) }
        for (i in n - 1 downTo 0) {
            for (j in m - 1 downTo 0) {
                max = max(grid[i][j], max)
                if (i == n - 1 && j == m - 1) {
                    continue
                }
                if (i == n - 1) {
                    dp[i][j] = grid[i][j + 1] + dp[i][j + 1]
                } else if (j == m - 1) {
                    dp[i][j] = grid[i + 1][j] + dp[i + 1][j]
                } else {
                    dp[i][j] = min(grid[i + 1][j] + dp[i + 1][j], grid[i][j + 1] + dp[i][j + 1])
                }
            }
        }
        val prev = IntArray(max + 1)
        prev.fill(Int.Companion.MAX_VALUE)
        for (i in 0..<n) {
            for (j in 0..<m) {
                prev[grid[i][j]] = min(prev[grid[i][j]], dp[i][j])
            }
        }
        for (i in 1..max) {
            prev[i] = min(prev[i], prev[i - 1])
        }
        for (tr in 1..k) {
            for (i in n - 1 downTo 0) {
                for (j in m - 1 downTo 0) {
                    if (i == n - 1 && j == m - 1) {
                        continue
                    }
                    dp[i][j] = prev[grid[i][j]]
                    if (i == n - 1) {
                        dp[i][j] = min(dp[i][j], grid[i][j + 1] + dp[i][j + 1])
                    } else if (j == m - 1) {
                        dp[i][j] = min(dp[i][j], grid[i + 1][j] + dp[i + 1][j])
                    } else {
                        dp[i][j] = min(dp[i][j], grid[i + 1][j] + dp[i + 1][j])
                        dp[i][j] = min(dp[i][j], grid[i][j + 1] + dp[i][j + 1])
                    }
                }
            }
            prev.fill(Int.Companion.MAX_VALUE)
            for (i in 0..<n) {
                for (j in 0..<m) {
                    prev[grid[i][j]] = min(prev[grid[i][j]], dp[i][j])
                }
            }
            for (i in 1..max) {
                prev[i] = min(prev[i], prev[i - 1])
            }
        }
        return dp[0][0]
    }
}
```