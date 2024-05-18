[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3148\. Maximum Difference Score in a Grid

Medium

You are given an `m x n` matrix `grid` consisting of **positive** integers. You can move from a cell in the matrix to **any** other cell that is either to the bottom or to the right (not necessarily adjacent). The score of a move from a cell with the value `c1` to a cell with the value `c2` is `c2 - c1`.

You can start at **any** cell, and you have to make **at least** one move.

Return the **maximum** total score you can achieve.

**Example 1:**

![](https://assets.leetcode.com/uploads/2024/03/14/grid1.png)

**Input:** grid = \[\[9,5,7,3],[8,9,6,1],[6,7,14,3],[2,5,3,1]]

**Output:** 9

**Explanation:** We start at the cell `(0, 1)`, and we perform the following moves:   

- Move from the cell `(0, 1)` to `(2, 1)` with a score of `7 - 5 = 2`.   

- Move from the cell `(2, 1)` to `(2, 2)` with a score of `14 - 7 = 7`.   

The total score is `2 + 7 = 9`.

**Example 2:**

![](https://assets.leetcode.com/uploads/2024/04/08/moregridsdrawio-1.png)

**Input:** grid = \[\[4,3,2],[3,2,1]]

**Output:** \-1

**Explanation:** We start at the cell `(0, 0)`, and we perform one move: `(0, 0)` to `(0, 1)`. The score is `3 - 4 = -1`.

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   `2 <= m, n <= 1000`
*   <code>4 <= m * n <= 10<sup>5</sup></code>
*   <code>1 <= grid[i][j] <= 10<sup>5</sup></code>

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun maxScore(grid: List<List<Int>>): Int {
        val m = grid.size - 1
        var row = grid[m]
        var n = row.size
        val maxRB = IntArray(n--)
        maxRB[n] = row[n]
        var mx = maxRB[n]
        var result = Int.MIN_VALUE
        for (i in n - 1 downTo 0) {
            val x = row[i]
            result = max(result, (mx - x))
            mx = max(mx, x)
            maxRB[i] = mx
        }
        for (i in m - 1 downTo 0) {
            row = grid[i]
            mx = 0
            for (j in n downTo 0) {
                mx = max(mx, maxRB[j])
                val x = row[j]
                result = max(result, (mx - x))
                mx = max(mx, x)
                maxRB[j] = mx
            }
        }
        return result
    }
}
```