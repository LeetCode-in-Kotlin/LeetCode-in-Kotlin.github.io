[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 73\. Set Matrix Zeroes

Medium

Given an `m x n` integer matrix `matrix`, if an element is `0`, set its entire row and column to `0`'s.

You must do it [in place](https://en.wikipedia.org/wiki/In-place_algorithm).

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/17/mat1.jpg)

**Input:** matrix = \[\[1,1,1],[1,0,1],[1,1,1]]

**Output:** [[1,0,1],[0,0,0],[1,0,1]]

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/08/17/mat2.jpg)

**Input:** matrix = \[\[0,1,2,0],[3,4,5,2],[1,3,1,5]]

**Output:** [[0,0,0,0],[0,4,5,0],[0,3,1,0]]

**Constraints:**

*   `m == matrix.length`
*   `n == matrix[0].length`
*   `1 <= m, n <= 200`
*   <code>-2<sup>31</sup> <= matrix[i][j] <= 2<sup>31</sup> - 1</code>

**Follow up:**

*   A straightforward solution using `O(mn)` space is probably a bad idea.
*   A simple improvement uses `O(m + n)` space, but still not the best solution.
*   Could you devise a constant space solution?

## Solution

```kotlin
class Solution {
    // Approach: Use first row and first column for storing whether in future
    //           the entire row or column needs to be marked 0
    fun setZeroes(matrix: Array<IntArray>) {
        val m = matrix.size
        val n: Int = matrix[0].size
        var row0 = false
        var col0 = false
        // Check if 0th col needs to be market all 0s in future
        for (ints in matrix) {
            if (ints[0] == 0) {
                col0 = true
                break
            }
        }
        // Check if 0th row needs to be market all 0s in future
        for (i in 0 until n) {
            if (matrix[0][i] == 0) {
                row0 = true
                break
            }
        }
        // Store the signals in 0th row and column
        for (i in 1 until m) {
            for (j in 1 until n) {
                if (matrix[i][j] == 0) {
                    matrix[i][0] = 0
                    matrix[0][j] = 0
                }
            }
        }
        // Mark 0 for all cells based on signal from 0th row and 0th column
        for (i in 1 until m) {
            for (j in 1 until n) {
                if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                    matrix[i][j] = 0
                }
            }
        }
        // Set 0th column
        for (i in 0 until m) {
            if (col0) {
                matrix[i][0] = 0
            }
        }
        // Set 0th row
        for (i in 0 until n) {
            if (row0) {
                matrix[0][i] = 0
            }
        }
    }
}
```