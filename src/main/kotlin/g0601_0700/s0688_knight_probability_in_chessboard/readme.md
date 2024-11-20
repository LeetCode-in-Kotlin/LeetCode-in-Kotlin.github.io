[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 688\. Knight Probability in Chessboard

Medium

On an `n x n` chessboard, a knight starts at the cell `(row, column)` and attempts to make exactly `k` moves. The rows and columns are **0-indexed**, so the top-left cell is `(0, 0)`, and the bottom-right cell is `(n - 1, n - 1)`.

A chess knight has eight possible moves it can make, as illustrated below. Each move is two cells in a cardinal direction, then one cell in an orthogonal direction.

![](https://assets.leetcode.com/uploads/2018/10/12/knight.png)

Each time the knight is to move, it chooses one of eight possible moves uniformly at random (even if the piece would go off the chessboard) and moves there.

The knight continues moving until it has made exactly `k` moves or has moved off the chessboard.

Return _the probability that the knight remains on the board after it has stopped moving_.

**Example 1:**

**Input:** n = 3, k = 2, row = 0, column = 0

**Output:** 0.06250

**Explanation:** There are two moves (to (1,2), (2,1)) that will keep the knight on the board. From each of those positions, there are also two moves that will keep the knight on the board. The total probability the knight stays on the board is 0.0625.

**Example 2:**

**Input:** n = 1, k = 0, row = 0, column = 0

**Output:** 1.00000

**Constraints:**

*   `1 <= n <= 25`
*   `0 <= k <= 100`
*   `0 <= row, column <= n - 1`

## Solution

```kotlin
class Solution {
    private val directions = arrayOf(
        intArrayOf(-2, -1),
        intArrayOf(-2, 1),
        intArrayOf(-1, 2),
        intArrayOf(1, 2),
        intArrayOf(2, -1),
        intArrayOf(2, 1),
        intArrayOf(1, -2),
        intArrayOf(-1, -2),
    )
    private lateinit var probabilityGiven: Array<Array<DoubleArray>>
    fun knightProbability(n: Int, k: Int, row: Int, column: Int): Double {
        probabilityGiven = Array(n) {
            Array(n) {
                DoubleArray(
                    k + 1,
                )
            }
        }
        return probability(row, column, k, n)
    }

    private fun probability(row: Int, column: Int, k: Int, n: Int): Double {
        return if (k == 0) {
            1.0
        } else if (probabilityGiven[row][column][k] != 0.0) {
            probabilityGiven[row][column][k]
        } else {
            var p = 0.0
            for (dir in directions) {
                if (isValid(row + dir[0], column + dir[1], n)) {
                    p += probability(row + dir[0], column + dir[1], k - 1, n)
                }
            }
            probabilityGiven[row][column][k] = p / 8.0
            probabilityGiven[row][column][k]
        }
    }

    private fun isValid(row: Int, column: Int, n: Int): Boolean {
        return row in 0 until n && column >= 0 && column < n
    }
}
```