[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3797\. Count Routes to Climb a Rectangular Grid

Hard

You are given a string array `grid` of size `n`, where each string `grid[i]` has length `m`. The character `grid[i][j]` is one of the following symbols:

*   `'.'`: The cell is available.
*   `'#'`: The cell is blocked.

You want to count the number of different routes to climb `grid`. Each route must start from _any cell_ in the bottom row (row `n - 1`) and end in the top row (row 0).

However, there are some constraints on the route.

*   You can only move from one available cell to **another** available cell.
*   The **Euclidean distance** of each move is **at most** `d`, where `d` is an integer parameter given to you. The Euclidean distance between two cells `(r1, c1)`, `(r2, c2)` is <code>sqrt((r1 - r2)<sup>2</sup> + (c1 - c2)<sup>2</sup>)</code>.
*   Each move either stays on the same row or moves to the row directly above (from row `r` to `r - 1`).
*   You cannot stay on the same row for two consecutive turns. If you stay on the same row in a move (and this move is not the last move), your next move must go to the row above.

Return an integer denoting the number of such routes. Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** grid = ["..","#."], d = 1

**Output:** 2

**Explanation:**

We label the cells we visit in the routes sequentially, starting from 1. The two routes are:

.2 #1

32 #1

We can move from the cell (1, 1) to the cell (0, 1) because the Euclidean distance is <code>sqrt((1 - 0)<sup>2</sup> + (1 - 1)<sup>2</sup>) = sqrt(1) <= d</code>.

However, we cannot move from the cell (1, 1) to the cell (0, 0) because the Euclidean distance is <code>sqrt((1 - 0)<sup>2</sup> + (1 - 0)<sup>2</sup>) = sqrt(2) > d</code>.

**Example 2:**

**Input:** grid = ["..","#."], d = 2

**Output:** 4

**Explanation:**

Two of the routes are given in example 1. The other two routes are:

2\. #1

23 #1

Note that we can move from (1, 1) to (0, 0) because the Euclidean distance is `sqrt(2) <= d`.

**Example 3:**

**Input:** grid = ["#"], d = 750

**Output:** 0

**Explanation:**

We cannot choose any cell as the starting cell. Therefore, there are no routes.

**Example 4:**

**Input:** grid = [".."], d = 1

**Output:** 4

**Explanation:**

The possible routes are:

.1

1\.

12

21

**Constraints:**

*   `1 <= n == grid.length <= 750`
*   `1 <= m == grid[i].length <= 750`
*   `grid[i][j]` is `'.'` or `'#'`.
*   `1 <= d <= 750`

## Solution

```kotlin
import kotlin.math.min
import kotlin.math.sqrt

class Solution {
    fun numberOfRoutes(grid: Array<String>, d: Int): Int {
        val n = grid.size
        val m = grid[0].length
        val mod: Long = 1000000007
        var dp: LongArray? = null
        for (i in n - 1 downTo 0) {
            val r = grid[i]
            if (dp == null) {
                val init = LongArray(m)
                init.fill(1)
                dp = f(init, 0, r, m, mod)
            } else {
                val d2 = sqrt(d.toDouble() * d - 1).toInt()
                dp = f(dp, d2, r, m, mod)
            }
            dp = f(dp, d, r, m, mod)
        }
        var res: Long = 0
        for (v in dp!!) {
            res = (res + v) % mod
        }
        return (res).toInt()
    }

    private fun f(dp: LongArray, dist: Int, r: String, m: Int, mod: Long): LongArray {
        val dp2 = LongArray(m)
        var window: Long = 0
        for (k in 0..min(m - 1, dist)) {
            window += dp[k]
        }
        dp2[0] = window
        for (j in 1..<m) {
            window = dp2[j - 1]
            if (j - dist - 1 >= 0) {
                window -= dp[j - dist - 1]
            }
            if (j + dist < m) {
                window += dp[j + dist]
            }
            dp2[j] = window
        }
        for (j in 0..<m) {
            dp2[j] = if (r[j] == '#') 0 else dp2[j] % mod
        }
        return dp2
    }
}
```