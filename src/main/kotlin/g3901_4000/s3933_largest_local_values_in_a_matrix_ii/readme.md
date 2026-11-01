[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3933\. Largest Local Values in a Matrix II

Medium

You are given an `n x m` integer matrix `matrix` containing non-negative integers.

A **non-zero** cell `(row, col)` checks the cells near it as follows:

*   Let `x = matrix[row][col]`.
*   Consider every cell within `x` rows and `x` columns of `(row, col)`.
*   Ignore cells that are outside the matrix.
*   Ignore the cells where both the row distance and column distance are exactly `x`.

The cell `(row, col)` is a **local maximum** if it is **non-zero** and no considered cell has a value **greater than** `x`.

Return an integer denoting the number of **local maximums** in `matrix`.

**Example 1:**

**Input:** matrix = \[\[0,0,0,0,0,0,0],[0,0,0,0,0,0,0],[0,0,0,0,0,0,0],[0,0,0,2,0,0,0],[0,0,0,0,0,0,0],[0,0,0,0,0,0,0],[0,0,0,0,0,0,0]]

**Output:** 1

![](https://assets.leetcode.com/uploads/2026/05/13/chatgpt-image-may-14-2026-01_53_19-am.png)

**Explanation:**

*   For the non-zero cell `(3, 3)`, `x = matrix[3][3] = 2`.
*   The highlighted cells are the considered cells within `x` rows and `x` columns of `(3, 3)`.
*   The four cells with both row and column distances equal to `x = 2` are ignored.
*   No considered cell has a value greater than 2, so `(3, 3)` is a local maximum.
*   There are no other non-zero cells, so the answer is 1.

**Example 2:**

**Input:** matrix = \[\[1,2],[3,4]]

**Output:** 1

**Explanation:**

Only the cell with value 4 is a local maximum. Every other non-zero cell considers a cell with a greater value.

**Example 3:**

**Input:** matrix = \[\[1,0,1],[0,1,0],[1,0,1]]

**Output:** 5

**Explanation:**

*   For a cell with value 1, the considered cells are the cell itself and its 4-directionally adjacent cells that are inside the matrix.
*   Each of the five cells with value 1 only considers cells with values 0 or 1, so all five of them are local maximums.

**Example 4:**

**Input:** matrix = \[\[1,1],[1,1]]

**Output:** 4

**Explanation:**

All cells have the same value. Therefore, no cell considers another cell with a greater value, so all 4 cells are local maximums.

**Constraints:**

*   `1 <= n == matrix.length <= 200`
*   `1 <= m == matrix[i].length <= 200`
*   `0 <= matrix[i][j] <= 200`

## Solution

```kotlin
import kotlin.math.max
import kotlin.math.min

class Solution {
    fun countLocalMaximums(matrix: Array<IntArray>): Int {
        val n = matrix.size
        val m = matrix[0].size
        val wn = 32 - Integer.numberOfLeadingZeros(n)
        val wm = 32 - Integer.numberOfLeadingZeros(m)
        val st = buildSparseTable(matrix, n, m, wn, wm)
        return countMaximums(matrix, st, n, m)
    }

    private fun buildSparseTable(
        matrix: Array<IntArray>,
        n: Int,
        m: Int,
        wn: Int,
        wm: Int,
    ): Array<Array<Array<IntArray>>> {
        val st =
            Array<Array<Array<IntArray>>>(wn) { Array<Array<IntArray>>(wm) { Array<IntArray>(n) { IntArray(m) } } }
        st[0][0] = matrix
        buildHorizontalLevels(st, n, m, wm)
        buildVerticalLevels(st, n, m, wn, wm)
        return st
    }

    private fun buildHorizontalLevels(st: Array<Array<Array<IntArray>>>, n: Int, m: Int, wm: Int) {
        for (k2 in 1..<wm) {
            val half = 1 shl (k2 - 1)
            val width = 1 shl k2
            for (i in 0..<n) {
                for (j in 0..m - width) {
                    st[0][k2][i][j] = max(st[0][k2 - 1][i][j], st[0][k2 - 1][i][j + half])
                }
            }
        }
    }

    private fun buildVerticalLevels(st: Array<Array<Array<IntArray>>>, n: Int, m: Int, wn: Int, wm: Int) {
        for (k1 in 1..<wn) {
            val half = 1 shl (k1 - 1)
            val height = 1 shl k1
            for (k2 in 0..<wm) {
                val width = 1 shl k2
                for (i in 0..n - height) {
                    for (j in 0..m - width) {
                        st[k1][k2][i][j] = max(st[k1 - 1][k2][i][j], st[k1 - 1][k2][i + half][j])
                    }
                }
            }
        }
    }

    private fun countMaximums(matrix: Array<IntArray>, st: Array<Array<Array<IntArray>>>, n: Int, m: Int): Int {
        var ans = 0
        for (i in 0..<n) {
            for (j in 0..<m) {
                if (isLocalMaximum(matrix, st, i, j, n, m)) {
                    ans++
                }
            }
        }
        return ans
    }

    private fun isLocalMaximum(
        matrix: Array<IntArray>,
        st: Array<Array<Array<IntArray>>>,
        i: Int,
        j: Int,
        n: Int,
        m: Int,
    ): Boolean {
        val x = matrix[i][j]
        if (x == 0) {
            return false
        }
        val max1 = query(st, i - x, j - x + 1, i + x + 1, j + x, n, m)
        val max2 = query(st, i - x + 1, j - x, i + x, j + x + 1, n, m)
        return max(max1, max2) <= x
    }

    private fun query(st: Array<Array<Array<IntArray>>>, r1: Int, c1: Int, r2: Int, c2: Int, n: Int, m: Int): Int {
        var r1 = r1
        var c1 = c1
        var r2 = r2
        var c2 = c2
        r1 = max(r1, 0)
        c1 = max(c1, 0)
        r2 = min(r2, n)
        c2 = min(c2, m)
        val k1 = 31 - Integer.numberOfLeadingZeros(r2 - r1)
        val k2 = 31 - Integer.numberOfLeadingZeros(c2 - c1)
        val bottom = r2 - (1 shl k1)
        val right = c2 - (1 shl k2)
        return max(
            max(st[k1][k2][r1][c1], st[k1][k2][bottom][c1]),
            max(st[k1][k2][r1][right], st[k1][k2][bottom][right]),
        )
    }
}
```