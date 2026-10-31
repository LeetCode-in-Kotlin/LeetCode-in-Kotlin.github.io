[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3742\. Maximum Path Score in a Grid

Medium

You are given an `m x n` grid where each cell contains one of the values 0, 1, or 2. You are also given an integer `k`.

You start from the top-left corner `(0, 0)` and want to reach the bottom-right corner `(m - 1, n - 1)` by moving only **right** or **down**.

Each cell contributes a specific score and incurs an associated cost, according to their cell values:

*   0: adds 0 to your score and costs 0.
*   1: adds 1 to your score and costs 1.
*   2: adds 2 to your score and costs 1.

Return the **maximum** score achievable without exceeding a total cost of `k`, or -1 if no valid path exists.

**Note:** If you reach the last cell but the total cost exceeds `k`, the path is invalid.

**Example 1:**

**Input:** grid = \[\[0, 1],[2, 0]], k = 1

**Output:** 2

**Explanation:**

The optimal path is:

| Cell   | grid[i][j] | Score | Total Score | Cost | Total Cost |
|--------|------------|-------|-------------|------|------------|
| (0, 0) | 0          | 0     | 0           | 0    | 0          |
| (1, 0) | 2          | 2     | 2           | 1    | 1          |
| (1, 1) | 0          | 0     | 2           | 0    | 1          |

Thus, the maximum possible score is 2.

**Example 2:**

**Input:** grid = \[\[0, 1],[1, 2]], k = 1

**Output:** \-1

**Explanation:**

There is no path that reaches cell `(1, 1)` without exceeding cost k. Thus, the answer is -1.

**Constraints:**

*   `1 <= m, n <= 200`
*   <code>0 <= k <= 10<sup>3</sup></code>
*   `grid[0][0] == 0`
*   `0 <= grid[i][j] <= 2`

## Solution

```kotlin
import kotlin.math.max
import kotlin.math.min

class Solution {
    fun maxPathScore(grid: Array<IntArray>, k: Int): Int {
        val n = grid.size
        val m = grid[0].size
        val mxc = min(k + 1, n + m + 5)
        val dp = Array<Array<IntArray>>(n) { Array<IntArray>(m) { IntArray(mxc) } }
        for (i in 0..<n) {
            for (j in 0..<m) {
                for (c in 0..<mxc) {
                    dp[i][j][c] = -1
                }
            }
        }
        dp[0][0][0] = 0
        for (i in 0..<n) {
            for (j in 0..<m) {
                for (c in 0..<mxc) {
                    if (dp[i][j][c] == -1) {
                        continue
                    }
                    // move right
                    if (j + 1 < m) {
                        val cost = c + (if (grid[i][j + 1] > 0) 1 else 0)
                        if (cost < mxc) {
                            dp[i][j + 1][cost] = max(dp[i][j + 1][cost], dp[i][j][c] + grid[i][j + 1])
                        }
                    }
                    // move down
                    if (i + 1 < n) {
                        val cost = c + (if (grid[i + 1][j] > 0) 1 else 0)
                        if (cost < mxc) {
                            dp[i + 1][j][cost] = max(dp[i + 1][j][cost], dp[i][j][c] + grid[i + 1][j])
                        }
                    }
                }
            }
        }
        var ans = -1
        for (c in 0..<mxc) {
            ans = max(ans, dp[n - 1][m - 1][c])
        }
        return ans
    }
}
```