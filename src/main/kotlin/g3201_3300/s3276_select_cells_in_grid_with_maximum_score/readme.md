[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3276\. Select Cells in Grid With Maximum Score

Hard

You are given a 2D matrix `grid` consisting of positive integers.

You have to select _one or more_ cells from the matrix such that the following conditions are satisfied:

*   No two selected cells are in the **same** row of the matrix.
*   The values in the set of selected cells are **unique**.

Your score will be the **sum** of the values of the selected cells.

Return the **maximum** score you can achieve.

**Example 1:**

**Input:** grid = \[\[1,2,3],[4,3,2],[1,1,1]]

**Output:** 8

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/07/29/grid1drawio.png)

We can select the cells with values 1, 3, and 4 that are colored above.

**Example 2:**

**Input:** grid = \[\[8,7,6],[8,3,2]]

**Output:** 15

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/07/29/grid8_8drawio.png)

We can select the cells with values 7 and 8 that are colored above.

**Constraints:**

*   `1 <= grid.length, grid[i].length <= 10`
*   `1 <= grid[i][j] <= 100`

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun maxScore(grid: List<List<Int>>): Int {
        val n = grid.size
        val m = grid[0].size
        val arr = Array(n * m) { IntArray(2) }
        for (i in 0 until n) {
            val l = grid[i]
            for (j in l.indices) {
                arr[i * m + j][0] = l[j]
                arr[i * m + j][1] = i
            }
        }
        arr.sortWith { a: IntArray, b: IntArray -> b[0] - a[0] }
        var dp = IntArray(1 shl n)
        var i = 0
        while (i < arr.size) {
            val seen = BooleanArray(n)
            seen[arr[i][1]] = true
            val v = arr[i][0]
            i++
            while (i < arr.size && arr[i][0] == v) {
                seen[arr[i][1]] = true
                i++
            }
            val next = dp.copyOf(dp.size)
            for (j in 0 until n) {
                if (seen[j]) {
                    val and = ((1 shl n) - 1) xor (1 shl j)
                    var k = and
                    while (k > 0) {
                        next[k or (1 shl j)] = max(next[k or (1 shl j)], (dp[k] + v))
                        k = (k - 1) and and
                    }
                    next[1 shl j] = max(next[1 shl j], v)
                }
            }
            dp = next
        }
        return dp[dp.size - 1]
    }
}
```