[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 529\. Minesweeper

Medium

Let's play the minesweeper game ([Wikipedia](https://en.wikipedia.org/wiki/Minesweeper_(video_game)), [online game](http://minesweeperonline.com))!

You are given an `m x n` char matrix `board` representing the game board where:

*   `'M'` represents an unrevealed mine,
*   `'E'` represents an unrevealed empty square,
*   `'B'` represents a revealed blank square that has no adjacent mines (i.e., above, below, left, right, and all 4 diagonals),
*   digit (`'1'` to `'8'`) represents how many mines are adjacent to this revealed square, and
*   `'X'` represents a revealed mine.

You are also given an integer array `click` where <code>click = [click<sub>r</sub>, click<sub>c</sub>]</code> represents the next click position among all the unrevealed squares (`'M'` or `'E'`).

Return _the board after revealing this position according to the following rules_:

1.  If a mine `'M'` is revealed, then the game is over. You should change it to `'X'`.
2.  If an empty square `'E'` with no adjacent mines is revealed, then change it to a revealed blank `'B'` and all of its adjacent unrevealed squares should be revealed recursively.
3.  If an empty square `'E'` with at least one adjacent mine is revealed, then change it to a digit (`'1'` to `'8'`) representing the number of adjacent mines.
4.  Return the board when no more squares will be revealed.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/10/12/minesweeper_example_1.png)

**Input:** board = \[\["E","E","E","E","E"],["E","E","M","E","E"],["E","E","E","E","E"],["E","E","E","E","E"]], click = [3,0]

**Output:** [["B","1","E","1","B"],["B","1","M","1","B"],["B","1","1","1","B"],["B","B","B","B","B"]]

**Example 2:**

![](https://assets.leetcode.com/uploads/2018/10/12/minesweeper_example_2.png)

**Input:** board = \[\["B","1","E","1","B"],["B","1","M","1","B"],["B","1","1","1","B"],["B","B","B","B","B"]], click = [1,2]

**Output:** [["B","1","E","1","B"],["B","1","X","1","B"],["B","1","1","1","B"],["B","B","B","B","B"]]

**Constraints:**

*   `m == board.length`
*   `n == board[i].length`
*   `1 <= m, n <= 50`
*   `board[i][j]` is either `'M'`, `'E'`, `'B'`, or a digit from `'1'` to `'8'`.
*   `click.length == 2`
*   <code>0 <= click<sub>r</sub> < m</code>
*   <code>0 <= click<sub>c</sub> < n</code>
*   <code>board[click<sub>r</sub>][click<sub>c</sub>]</code> is either `'M'` or `'E'`.

## Solution

```kotlin
class Solution {
    private var row = 0
    private var col = 0
    private fun dfs(board: Array<CharArray>, row: Int, col: Int) {
        if (row < 0 || row >= this.row || col < 0 || col >= this.col) {
            return
        }
        if (board[row][col] == 'E') {
            val numOfMine = bfs(board, row, col)
            if (numOfMine != 0) {
                board[row][col] = (numOfMine + '0'.code).toChar()
                return
            } else {
                board[row][col] = 'B'
            }
            for (i in DIRECTION) {
                dfs(board, row + i[0], col + i[1])
            }
        }
    }

    private fun bfs(board: Array<CharArray>, row: Int, col: Int): Int {
        var numOfMine = 0
        for (i in DIRECTION) {
            val newRow = row + i[0]
            val newCol = col + i[1]
            if (newRow >= 0 && newRow < this.row && newCol >= 0 && newCol < this.col && board[newRow][newCol] == 'M') {
                numOfMine++
            }
        }
        return numOfMine
    }

    fun updateBoard(board: Array<CharArray>, c: IntArray): Array<CharArray> {
        if (board[c[0]][c[1]] == 'M') {
            board[c[0]][c[1]] = 'X'
            return board
        } else {
            row = board.size
            col = board[0].size
            dfs(board, c[0], c[1])
        }
        return board
    }

    companion object {
        private val DIRECTION = arrayOf(
            intArrayOf(1, 0),
            intArrayOf(-1, 0),
            intArrayOf(0, 1),
            intArrayOf(0, -1),
            intArrayOf(-1, -1),
            intArrayOf(-1, 1),
            intArrayOf(1, -1),
            intArrayOf(1, 1)
        )
    }
}
```