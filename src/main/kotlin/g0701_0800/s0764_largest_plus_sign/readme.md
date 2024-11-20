[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 764\. Largest Plus Sign

Medium

You are given an integer `n`. You have an `n x n` binary grid `grid` with all values initially `1`'s except for some indices given in the array `mines`. The <code>i<sup>th</sup></code> element of the array `mines` is defined as <code>mines[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> where <code>grid[x<sub>i</sub>][y<sub>i</sub>] == 0</code>.

Return _the order of the largest **axis-aligned** plus sign of_ 1_'s contained in_ `grid`. If there is none, return `0`.

An **axis-aligned plus sign** of `1`'s of order `k` has some center `grid[r][c] == 1` along with four arms of length `k - 1` going up, down, left, and right, and made of `1`'s. Note that there could be `0`'s or `1`'s beyond the arms of the plus sign, only the relevant area of the plus sign is checked for `1`'s.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/06/13/plus1-grid.jpg)

**Input:** n = 5, mines = \[\[4,2]]

**Output:** 2

**Explanation:** In the above grid, the largest plus sign can only be of order 2. One of them is shown.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/06/13/plus2-grid.jpg)

**Input:** n = 1, mines = \[\[0,0]]

**Output:** 0

**Explanation:** There is no plus sign, so return 0.

**Constraints:**

*   `1 <= n <= 500`
*   `1 <= mines.length <= 5000`
*   <code>0 <= x<sub>i</sub>, y<sub>i</sub> < n</code>
*   All the pairs <code>(x<sub>i</sub>, y<sub>i</sub>)</code> are **unique**.

## Solution

```kotlin
class Solution {
    fun orderOfLargestPlusSign(n: Int, mines: Array<IntArray>): Int {
        val mat = Array(n) { BooleanArray(n) }
        for (pos in mines) {
            mat[pos[0]][pos[1]] = true
        }
        val left = Array(n) { IntArray(n) }
        val right = Array(n) { IntArray(n) }
        val up = Array(n) { IntArray(n) }
        val down = Array(n) { IntArray(n) }
        var ans = 0
        // For Left and Up only
        for (i in 0 until n) {
            for (j in 0 until n) {
                val i1 = if (j == 0) 0 else left[i][j - 1]
                left[i][j] = if (mat[i][j]) 0 else 1 + i1
                val i2 = if (i == 0) 0 else up[i - 1][j]
                up[i][j] = if (mat[i][j]) 0 else 1 + i2
            }
        }
        // For Right and Down and simoultaneously get answer
        for (i in n - 1 downTo 0) {
            for (j in n - 1 downTo 0) {
                val i1 = if (j == n - 1) 0 else right[i][j + 1]
                right[i][j] = if (mat[i][j]) 0 else 1 + i1
                val i2 = if (i == n - 1) 0 else down[i + 1][j]
                down[i][j] = if (mat[i][j]) 0 else 1 + i2
                val x = left[i][j].coerceAtMost(up[i][j]).coerceAtMost(
                    right[i][j].coerceAtMost(down[i][j]),
                )
                ans = ans.coerceAtLeast(x)
            }
        }
        return ans
    }
}
```