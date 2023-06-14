[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1582\. Special Positions in a Binary Matrix

Easy

Given an `m x n` binary matrix `mat`, return _the number of special positions in_ `mat`_._

A position `(i, j)` is called **special** if `mat[i][j] == 1` and all other elements in row `i` and column `j` are `0` (rows and columns are **0-indexed**).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/12/23/special1.jpg)

**Input:** mat = \[\[1,0,0],[0,0,1],[1,0,0]]

**Output:** 1

**Explanation:** (1, 2) is a special position because mat[1][2] == 1 and all other elements in row 1 and column 2 are 0.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/12/24/special-grid.jpg)

**Input:** mat = \[\[1,0,0],[0,1,0],[0,0,1]]

**Output:** 3

**Explanation:** (0, 0), (1, 1) and (2, 2) are special positions.

**Constraints:**

*   `m == mat.length`
*   `n == mat[i].length`
*   `1 <= m, n <= 100`
*   `mat[i][j]` is either `0` or `1`.

## Solution

```kotlin
class Solution {
    fun numSpecial(mat: Array<IntArray>): Int {
        var count = 0
        for (i in mat.indices) {
            for (j in mat[0].indices) {
                if (mat[i][j] == 1 && isSpecial(mat, i, j)) {
                    count++
                }
            }
        }
        return count
    }

    private fun isSpecial(mat: Array<IntArray>, row: Int, col: Int): Boolean {
        for (i in mat.indices) {
            if (i != row && mat[i][col] == 1) {
                return false
            }
        }
        for (j in mat[0].indices) {
            if (j != col && mat[row][j] == 1) {
                return false
            }
        }
        return true
    }
}
```