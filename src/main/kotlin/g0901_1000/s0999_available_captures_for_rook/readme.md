[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 999\. Available Captures for Rook

Easy

On an `8 x 8` chessboard, there is **exactly one** white rook `'R'` and some number of white bishops `'B'`, black pawns `'p'`, and empty squares `'.'`.

When the rook moves, it chooses one of four cardinal directions (north, east, south, or west), then moves in that direction until it chooses to stop, reaches the edge of the board, captures a black pawn, or is blocked by a white bishop. A rook is considered **attacking** a pawn if the rook can capture the pawn on the rook's turn. The **number of available captures** for the white rook is the number of pawns that the rook is **attacking**.

Return _the **number of available captures** for the white rook_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/02/20/1253_example_1_improved.PNG)

**Input:** board = \[\[".",".",".",".",".",".",".","."],[".",".",".","p",".",".",".","."],[".",".",".","R",".",".",".","p"],[".",".",".",".",".",".",".","."],[".",".",".",".",".",".",".","."],[".",".",".","p",".",".",".","."],[".",".",".",".",".",".",".","."],[".",".",".",".",".",".",".","."]]

**Output:** 3

**Explanation:** In this example, the rook is attacking all the pawns.

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/02/19/1253_example_2_improved.PNG)

**Input:** board = \[\[".",".",".",".",".",".",".","."],[".","p","p","p","p","p",".","."],[".","p","p","B","p","p",".","."],[".","p","B","R","B","p",".","."],[".","p","p","B","p","p",".","."],[".","p","p","p","p","p",".","."],[".",".",".",".",".",".",".","."],[".",".",".",".",".",".",".","."]]

**Output:** 0

**Explanation:** The bishops are blocking the rook from attacking any of the pawns.

**Example 3:**

![](https://assets.leetcode.com/uploads/2019/02/20/1253_example_3_improved.PNG)

**Input:** board = \[\[".",".",".",".",".",".",".","."],[".",".",".","p",".",".",".","."],[".",".",".","p",".",".",".","."],["p","p",".","R",".","p","B","."],[".",".",".",".",".",".",".","."],[".",".",".","B",".",".",".","."],[".",".",".","p",".",".",".","."],[".",".",".",".",".",".",".","."]]

**Output:** 3

**Explanation:** The rook is attacking the pawns at positions b5, d6, and f5.

**Constraints:**

*   `board.length == 8`
*   `board[i].length == 8`
*   `board[i][j]` is either `'R'`, `'.'`, `'B'`, or `'p'`
*   There is exactly one cell with `board[i][j] == 'R'`

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    private val directions = intArrayOf(0, 1, 0, -1, 0)
    fun numRookCaptures(board: Array<CharArray>): Int {
        val m = board.size
        val n = board[0].size
        var rowR = -1
        var colR = -1
        for (i in 0 until m) {
            for (j in 0 until n) {
                if (board[i][j] == 'R') {
                    rowR = i
                    colR = j
                    break
                }
            }
        }
        val count = intArrayOf(0)
        for (i in 0..3) {
            val neighborRow = rowR + directions[i]
            val neighborCol = colR + directions[i + 1]
            if (neighborRow in 0 until m && neighborCol >= 0 && neighborCol < n &&
                board[neighborRow][neighborCol] != 'B'
            ) {
                if (directions[i] == 0 && directions[i + 1] == 1) {
                    extracted(board, n, count, neighborRow, neighborCol)
                } else if (directions[i] == 1 && directions[i + 1] == 0) {
                    extracted1(board, m, count, neighborRow, neighborCol)
                } else if (directions[i] == 0 && directions[i + 1] == -1) {
                    extracted(board, count, neighborRow, neighborCol)
                } else {
                    extracted1(board, count, neighborRow, neighborCol)
                }
            }
        }
        return count[0]
    }

    private fun extracted(board: Array<CharArray>, count: IntArray, neighborRow: Int, neighborCol: Int) {
        var neighborCol = neighborCol
        while (neighborCol >= 0) {
            if (board[neighborRow][neighborCol] == 'p') {
                count[0]++
                break
            } else if (board[neighborRow][neighborCol] == 'B') {
                break
            } else {
                neighborCol--
            }
        }
    }

    private fun extracted(board: Array<CharArray>, n: Int, count: IntArray, neighborRow: Int, neighborCol: Int) {
        var neighborCol = neighborCol
        while (neighborCol < n) {
            if (board[neighborRow][neighborCol] == 'p') {
                count[0]++
                break
            } else if (board[neighborRow][neighborCol] == 'B') {
                break
            } else {
                neighborCol++
            }
        }
    }

    private fun extracted1(board: Array<CharArray>, count: IntArray, neighborRow: Int, neighborCol: Int) {
        var neighborRow = neighborRow
        while (neighborRow >= 0) {
            if (board[neighborRow][neighborCol] == 'p') {
                count[0]++
                break
            } else if (board[neighborRow][neighborCol] == 'B') {
                break
            } else {
                neighborRow--
            }
        }
    }

    private fun extracted1(board: Array<CharArray>, m: Int, count: IntArray, neighborRow: Int, neighborCol: Int) {
        var neighborRow = neighborRow
        while (neighborRow < m) {
            if (board[neighborRow][neighborCol] == 'p') {
                count[0]++
                break
            } else if (board[neighborRow][neighborCol] == 'B') {
                break
            } else {
                neighborRow++
            }
        }
    }
}
```