[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1314\. Matrix Block Sum

Medium

Given a `m x n` matrix `mat` and an integer `k`, return _a matrix_ `answer` _where each_ `answer[i][j]` _is the sum of all elements_ `mat[r][c]` _for_:

*   `i - k <= r <= i + k,`
*   `j - k <= c <= j + k`, and
*   `(r, c)` is a valid position in the matrix.

**Example 1:**

**Input:** mat = \[\[1,2,3],[4,5,6],[7,8,9]], k = 1

**Output:** [[12,21,16],[27,45,33],[24,39,28]]

**Example 2:**

**Input:** mat = \[\[1,2,3],[4,5,6],[7,8,9]], k = 2

**Output:** [[45,45,45],[45,45,45],[45,45,45]]

**Constraints:**

*   `m == mat.length`
*   `n == mat[i].length`
*   `1 <= m, n, k <= 100`
*   `1 <= mat[i][j] <= 100`

## Solution

```kotlin
class Solution {
    fun matrixBlockSum(mat: Array<IntArray>, k: Int): Array<IntArray> {
        val rows = mat.size
        val cols = mat[0].size
        val prefixSum = Array(rows + 1) { IntArray(cols + 1) }
        for (i in 1..rows) {
            for (j in 1..cols) {
                prefixSum[i][j] = (
                    (
                        mat[i - 1][j - 1] -
                            prefixSum[i - 1][j - 1]
                        ) + prefixSum[i - 1][j] +
                        prefixSum[i][j - 1]
                    )
            }
        }
        val result = Array(rows) { IntArray(cols) }
        for (i in 0 until rows) {
            for (j in 0 until cols) {
                val iMin = Math.max(i - k, 0)
                val iMax = Math.min(i + k, rows - 1)
                val jMin = Math.max(j - k, 0)
                val jMax = Math.min(j + k, cols - 1)
                result[i][j] = (
                    (
                        prefixSum[iMin][jMin] +
                            prefixSum[iMax + 1][jMax + 1]
                        ) - prefixSum[iMax + 1][jMin] -
                        prefixSum[iMin][jMax + 1]
                    )
            }
        }
        return result
    }
}
```