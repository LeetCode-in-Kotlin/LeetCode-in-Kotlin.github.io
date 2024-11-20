[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 52\. N-Queens II

Hard

The **n-queens** puzzle is the problem of placing `n` queens on an `n x n` chessboard such that no two queens attack each other.

Given an integer `n`, return _the number of distinct solutions to the **n-queens puzzle**_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/13/queens.jpg)

**Input:** n = 4

**Output:** 2

**Explanation:** There are two distinct solutions to the 4-queens puzzle as shown.

**Example 2:**

**Input:** n = 1

**Output:** 1

**Constraints:**

*   `1 <= n <= 9`

## Solution

```kotlin
class Solution {
    fun totalNQueens(n: Int): Int {
        val row = BooleanArray(n)
        val col = BooleanArray(n)
        val diagonal = BooleanArray(n + n - 1)
        val antiDiagonal = BooleanArray(n + n - 1)
        return totalNQueens(n, 0, row, col, diagonal, antiDiagonal)
    }

    private fun totalNQueens(
        n: Int,
        r: Int,
        row: BooleanArray,
        col: BooleanArray,
        diagonal: BooleanArray,
        antiDiagonal: BooleanArray,
    ): Int {
        if (r == n) {
            return 1
        }
        var count = 0
        for (c in 0 until n) {
            if (!row[r] && !col[c] && !diagonal[r + c] && !antiDiagonal[r - c + n - 1]) {
                antiDiagonal[r - c + n - 1] = true
                diagonal[r + c] = antiDiagonal[r - c + n - 1]
                col[c] = diagonal[r + c]
                row[r] = col[c]
                count += totalNQueens(n, r + 1, row, col, diagonal, antiDiagonal)
                antiDiagonal[r - c + n - 1] = false
                diagonal[r + c] = antiDiagonal[r - c + n - 1]
                col[c] = diagonal[r + c]
                row[r] = col[c]
            }
        }
        return count
    }
}
```