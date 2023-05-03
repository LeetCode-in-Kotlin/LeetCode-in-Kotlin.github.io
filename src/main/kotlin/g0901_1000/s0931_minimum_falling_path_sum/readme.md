[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 931\. Minimum Falling Path Sum

Medium

Given an `n x n` array of integers `matrix`, return _the **minimum sum** of any **falling path** through_ `matrix`.

A **falling path** starts at any element in the first row and chooses the element in the next row that is either directly below or diagonally left/right. Specifically, the next element from position `(row, col)` will be `(row + 1, col - 1)`, `(row + 1, col)`, or `(row + 1, col + 1)`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/11/03/failing1-grid.jpg)

**Input:** matrix = \[\[2,1,3],[6,5,4],[7,8,9]]

**Output:** 13

**Explanation:** There are two falling paths with a minimum sum as shown.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/11/03/failing2-grid.jpg)

**Input:** matrix = \[\[-19,57],[-40,-5]]

**Output:** -59

**Explanation:** The falling path with a minimum sum is shown.

**Constraints:**

*   `n == matrix.length == matrix[i].length`
*   `1 <= n <= 100`
*   `-100 <= matrix[i][j] <= 100`

## Solution

```kotlin
class Solution {
    fun minFallingPathSum(matrix: Array<IntArray>): Int {
        val l = matrix[0].size
        var arr = matrix[0]
        for (i in 1 until matrix.size) {
            val cur = IntArray(l)
            for (j in 0 until l) {
                var left = Int.MAX_VALUE
                var right = Int.MAX_VALUE
                val curCell = arr[j]
                if (j > 0) {
                    left = arr[j - 1]
                }
                if (j < l - 1) {
                    right = arr[j + 1]
                }
                cur[j] = matrix[i][j] + left.coerceAtMost(right.coerceAtMost(curCell))
            }
            arr = cur
        }
        var min = Int.MAX_VALUE
        for (i in 0 until l) {
            if (arr[i] < min) {
                min = arr[i]
            }
        }
        return min
    }
}
```