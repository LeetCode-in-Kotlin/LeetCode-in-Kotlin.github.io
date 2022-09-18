[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 37\. Sudoku Solver

Hard

Write a program to solve a Sudoku puzzle by filling the empty cells.

A sudoku solution must satisfy **all of the following rules**:

1.  Each of the digits `1-9` must occur exactly once in each row.
2.  Each of the digits `1-9` must occur exactly once in each column.
3.  Each of the digits `1-9` must occur exactly once in each of the 9 `3x3` sub-boxes of the grid.

The `'.'` character indicates empty cells.

**Example 1:**

![](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)

**Input:** board = \[\["5","3",".",".","7",".",".",".","."],

["6",".",".","1","9","5",".",".","."],

[".","9","8",".",".",".",".","6","."],

["8",".",".",".","6",".",".",".","3"],

["4",".",".","8",".","3",".",".","1"],

["7",".",".",".","2",".",".",".","6"],

[".","6",".",".",".",".","2","8","."],

[".",".",".","4","1","9",".",".","5"],

[".",".",".",".","8",".",".","7","9"]]

**Output:** [["5","3","4","6","7","8","9","1","2"],

["6","7","2","1","9","5","3","4","8"],

["1","9","8","3","4","2","5","6","7"],

["8","5","9","7","6","1","4","2","3"],

["4","2","6","8","5","3","7","9","1"],

["7","1","3","9","2","4","8","5","6"],

["9","6","1","5","3","7","2","8","4"],

["2","8","7","4","1","9","6","3","5"],

["3","4","5","2","8","6","1","7","9"]]

**Explanation:** The input board is shown above and the only valid solution is shown below: ![](https://upload.wikimedia.org/wikipedia/commons/thumb/3/31/Sudoku-by-L2G-20050714_solution.svg/250px-Sudoku-by-L2G-20050714_solution.svg.png)

**Constraints:**

*   `board.length == 9`
*   `board[i].length == 9`
*   `board[i][j]` is a digit or `'.'`.
*   It is **guaranteed** that the input board has only one solution.

## Solution

```kotlin
class Solution {
    private val emptyCells: MutableList<IntArray> = ArrayList()
    private val rows = IntArray(9)
    private val cols = IntArray(9)
    private val boxes = IntArray(9)

    fun solveSudoku(board: Array<CharArray>) {
        for (r in 0..8) {
            for (c in 0..8) {
                if (board[r][c] == '.') {
                    emptyCells.add(intArrayOf(r, c))
                } else {
                    val `val` = board[r][c] - '0'
                    val boxPos = r / 3 * 3 + c / 3
                    rows[r] = rows[r] or (1 shl `val`)
                    cols[c] = cols[c] or (1 shl `val`)
                    boxes[boxPos] = boxes[boxPos] or (1 shl `val`)
                }
            }
        }
        backtracking(board, 0)
    }

    private fun backtracking(board: Array<CharArray>, i: Int): Boolean {
        // Check if we filled all empty cells?
        if (i == emptyCells.size) {
            return true
        }
        val r = emptyCells[i][0]
        val c = emptyCells[i][1]
        val boxPos = r / 3 * 3 + c / 3
        for (`val` in 1..9) {
            // skip if that value is existed!
            if (hasBit(rows[r], `val`) || hasBit(cols[c], `val`) || hasBit(boxes[boxPos], `val`)) {
                continue
            }
            board[r][c] = ('0' + `val`)
            // backup old values
            val oldRow = rows[r]
            val oldCol = cols[c]
            val oldBox = boxes[boxPos]
            rows[r] = rows[r] or (1 shl `val`)
            cols[c] = cols[c] or (1 shl `val`)
            boxes[boxPos] = boxes[boxPos] or (1 shl `val`)
            if (backtracking(board, i + 1)) {
                return true
            }
            rows[r] = oldRow
            cols[c] = oldCol
            boxes[boxPos] = oldBox
        }
        return false
    }

    private fun hasBit(x: Int, k: Int): Boolean {
        return x shr k and 1 == 1
    }
}
```