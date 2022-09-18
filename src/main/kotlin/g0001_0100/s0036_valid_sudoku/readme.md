[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 36\. Valid Sudoku

Medium

Determine if a `9 x 9` Sudoku board is valid. Only the filled cells need to be validated **according to the following rules**:

1.  Each row must contain the digits `1-9` without repetition.
2.  Each column must contain the digits `1-9` without repetition.
3.  Each of the nine `3 x 3` sub-boxes of the grid must contain the digits `1-9` without repetition.

**Note:**

*   A Sudoku board (partially filled) could be valid but is not necessarily solvable.
*   Only the filled cells need to be validated according to the mentioned rules.

**Example 1:**

![](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)

**Input:** board = 

[["5","3",".",".","7",".",".",".","."] ,

["6",".",".","1","9","5",".",".","."] ,

[".","9","8",".",".",".",".","6","."] ,

["8",".",".",".","6",".",".",".","3"] ,

["4",".",".","8",".","3",".",".","1"] ,

["7",".",".",".","2",".",".",".","6"] ,

[".","6",".",".",".",".","2","8","."] ,

[".",".",".","4","1","9",".",".","5"] ,

[".",".",".",".","8",".",".","7","9"]]

**Output:** true

**Example 2:**

**Input:** board = 

[["8","3",".",".","7",".",".",".","."] ,

["6",".",".","1","9","5",".",".","."] ,

[".","9","8",".",".",".",".","6","."] ,

["8",".",".",".","6",".",".",".","3"] ,

["4",".",".","8",".","3",".",".","1"] ,

["7",".",".",".","2",".",".",".","6"] ,

[".","6",".",".",".",".","2","8","."] ,

[".",".",".","4","1","9",".",".","5"] ,

[".",".",".",".","8",".",".","7","9"]]

**Output:** false

**Explanation:** Same as Example 1, except with the **5** in the top left corner being modified to **8**. Since there are two 8's in the top left 3x3 sub-box, it is invalid.

**Constraints:**

*   `board.length == 9`
*   `board[i].length == 9`
*   `board[i][j]` is a digit `1-9` or `'.'`.

## Solution

```kotlin
class Solution {
    private var j1 = 0
    private val i1 = IntArray(9)
    private val b1 = IntArray(9)

    fun isValidSudoku(board: Array<CharArray>): Boolean {
        for (i in 0..8) {
            for (j in 0..8) {
                val res = checkValid(board, i, j)
                if (!res) {
                    return false
                }
            }
        }
        return true
    }

    private fun checkValid(board: Array<CharArray>, i: Int, j: Int): Boolean {
        if (j == 0) {
            j1 = 0
        }
        if (board[i][j] == '.') {
            return true
        }
        val `val` = board[i][j] - '0'
        if (j1 == j1 or (1 shl `val` - 1)) {
            return false
        }
        j1 = j1 or (1 shl `val` - 1)
        if (i1[j] == i1[j] or (1 shl `val` - 1)) {
            return false
        }
        i1[j] = i1[j] or (1 shl `val` - 1)
        val b = i / 3 * 3 + j / 3
        if (b1[b] == b1[b] or (1 shl `val` - 1)) {
            return false
        }
        b1[b] = b1[b] or (1 shl `val` - 1)
        return true
    }
}
```