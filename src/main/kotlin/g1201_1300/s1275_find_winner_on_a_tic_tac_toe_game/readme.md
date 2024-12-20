[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1275\. Find Winner on a Tic Tac Toe Game

Easy

**Tic-tac-toe** is played by two players `A` and `B` on a `3 x 3` grid. The rules of Tic-Tac-Toe are:

*   Players take turns placing characters into empty squares `' '`.
*   The first player `A` always places `'X'` characters, while the second player `B` always places `'O'` characters.
*   `'X'` and `'O'` characters are always placed into empty squares, never on filled ones.
*   The game ends when there are **three** of the same (non-empty) character filling any row, column, or diagonal.
*   The game also ends if all squares are non-empty.
*   No more moves can be played if the game is over.

Given a 2D integer array `moves` where <code>moves[i] = [row<sub>i</sub>, col<sub>i</sub>]</code> indicates that the <code>i<sup>th</sup></code> move will be played on <code>grid[row<sub>i</sub>][col<sub>i</sub>]</code>. return _the winner of the game if it exists_ (`A` or `B`). In case the game ends in a draw return `"Draw"`. If there are still movements to play return `"Pending"`.

You can assume that `moves` is valid (i.e., it follows the rules of **Tic-Tac-Toe**), the grid is initially empty, and `A` will play first.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/09/22/xo1-grid.jpg)

**Input:** moves = \[\[0,0],[2,0],[1,1],[2,1],[2,2]]

**Output:** "A"

**Explanation:** A wins, they always play first.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/09/22/xo2-grid.jpg)

**Input:** moves = \[\[0,0],[1,1],[0,1],[0,2],[1,0],[2,0]]

**Output:** "B"

**Explanation:** B wins.

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/09/22/xo3-grid.jpg)

**Input:** moves = \[\[0,0],[1,1],[2,0],[1,0],[1,2],[2,1],[0,1],[0,2],[2,2]]

**Output:** "Draw"

**Explanation:** The game ends in a draw since there are no moves to make.

**Constraints:**

*   `1 <= moves.length <= 9`
*   `moves[i].length == 2`
*   <code>0 <= row<sub>i</sub>, col<sub>i</sub> <= 2</code>
*   There are no repeated elements on `moves`.
*   `moves` follow the rules of tic tac toe.

## Solution

```kotlin
class Solution {
    fun tictactoe(moves: Array<IntArray>): String {
        val board = Array(3) { arrayOfNulls<String>(3) }
        for (i in moves.indices) {
            if (i % 2 == 0) {
                board[moves[i][0]][moves[i][1]] = "X"
            } else {
                board[moves[i][0]][moves[i][1]] = "O"
            }
            if (i > 3 && wins(board) != "") {
                return wins(board)
            }
        }
        return if (moves.size == 9) "Draw" else "Pending"
    }

    private fun wins(board: Array<Array<String?>>): String {
        for (i in 0..2) {
            if (board[i][0] == null) {
                continue
            }
            val str = board[i][0]
            if (str == board[i][1] && str == board[i][2]) {
                return getWinner(str)
            }
        }
        for (j in 0..2) {
            if (board[0][j] == null) {
                continue
            }
            val str = board[0][j]
            if (str == board[1][j] && str == board[2][j]) {
                return getWinner(str)
            }
        }
        if (board[1][1] == null) {
            return ""
        }
        val str = board[1][1]
        return if (str == board[0][0] && str == board[2][2] || str == board[0][2] && str == board[2][0]) {
            getWinner(str)
        } else {
            ""
        }
    }

    private fun getWinner(str: String?): String {
        return if (str == "X") {
            "A"
        } else {
            "B"
        }
    }
}
```