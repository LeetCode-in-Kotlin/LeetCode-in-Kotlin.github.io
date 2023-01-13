[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 498\. Diagonal Traverse

Medium

Given an `m x n` matrix `mat`, return _an array of all the elements of the array in a diagonal order_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/10/diag1-grid.jpg)

**Input:** mat = \[\[1,2,3],[4,5,6],[7,8,9]]

**Output:** [1,2,4,7,5,3,6,8,9]

**Example 2:**

**Input:** mat = \[\[1,2],[3,4]]

**Output:** [1,2,3,4]

**Constraints:**

*   `m == mat.length`
*   `n == mat[i].length`
*   <code>1 <= m, n <= 10<sup>4</sup></code>
*   <code>1 <= m * n <= 10<sup>4</sup></code>
*   <code>-10<sup>5</sup> <= mat[i][j] <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    fun findDiagonalOrder(mat: Array<IntArray>): IntArray {
        val m = mat.size
        val n = mat[0].size
        val output = IntArray(m * n)
        var idx = 0
        for (diag in 0..m + n - 2) {
            if (diag % 2 == 0) {
                for (k in Math.max(0, diag - m + 1)..Math.min(diag, n - 1)) {
                    output[idx++] = mat[diag - k][k]
                }
            } else {
                for (k in Math.max(0, diag - n + 1)..Math.min(diag, m - 1)) {
                    output[idx++] = mat[k][diag - k]
                }
            }
        }
        return output
    }
}
```