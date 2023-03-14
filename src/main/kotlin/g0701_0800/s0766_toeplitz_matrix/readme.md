[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 766\. Toeplitz Matrix

Easy

Given an `m x n` `matrix`, return _`true` if the matrix is Toeplitz. Otherwise, return `false`._

A matrix is **Toeplitz** if every diagonal from top-left to bottom-right has the same elements.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/04/ex1.jpg)

**Input:** matrix = \[\[1,2,3,4],[5,1,2,3],[9,5,1,2]]

**Output:** true

**Explanation:** In the above grid, the diagonals are: "[9]", "[5, 5]", "[1, 1, 1]", "[2, 2, 2]", "[3, 3]", "[4]". In each diagonal all elements are the same, so the answer is True.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/04/ex2.jpg)

**Input:** matrix = \[\[1,2],[2,2]]

**Output:** false

**Explanation:** The diagonal "[1, 2]" has different elements.

**Constraints:**

*   `m == matrix.length`
*   `n == matrix[i].length`
*   `1 <= m, n <= 20`
*   `0 <= matrix[i][j] <= 99`

**Follow up:**

*   What if the `matrix` is stored on disk, and the memory is limited such that you can only load at most one row of the matrix into the memory at once?
*   What if the `matrix` is so large that you can only load up a partial row into the memory at once?

## Solution

```kotlin
class Solution {
    fun isToeplitzMatrix(matrix: Array<IntArray>): Boolean {
        val m = matrix.size
        val n = matrix[0].size
        var i = 0
        var j = 0
        var sameVal = matrix[i][j]
        while (++i < m && ++j < n) {
            if (matrix[i][j] != sameVal) {
                return false
            }
        }
        i = 1
        j = 0
        while (i < m) {
            var tmpI = i
            var tmpJ = j
            sameVal = matrix[i][j]
            while (++tmpI < m && ++tmpJ < n) {
                if (matrix[tmpI][tmpJ] != sameVal) {
                    return false
                }
            }
            i++
        }
        i = 0
        j = 1
        while (j < n) {
            var tmpJ = j
            var tmpI = i
            sameVal = matrix[tmpI][tmpJ]
            while (++tmpI < m && ++tmpJ < n) {
                if (matrix[tmpI][tmpJ] != sameVal) {
                    return false
                }
            }
            j++
        }
        return true
    }
}
```