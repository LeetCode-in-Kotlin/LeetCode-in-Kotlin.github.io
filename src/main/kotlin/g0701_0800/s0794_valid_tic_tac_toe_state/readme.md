[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 794\. Valid Tic-Tac-Toe State

Medium

Given a Tic-Tac-Toe board as a string array `board`, return `true` if and only if it is possible to reach this board position during the course of a valid tic-tac-toe game.

The board is a `3 x 3` array that consists of characters `' '`, `'X'`, and `'O'`. The `' '` character represents an empty square.

Here are the rules of Tic-Tac-Toe:

*   Players take turns placing characters into empty squares `' '`.
*   The first player always places `'X'` characters, while the second player always places `'O'` characters.
*   `'X'` and `'O'` characters are always placed into empty squares, never filled ones.
*   The game ends when there are three of the same (non-empty) character filling any row, column, or diagonal.
*   The game also ends if all squares are non-empty.
*   No more moves can be played if the game is over.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/05/15/tictactoe1-grid.jpg)

**Input:** board = ["O "," "," "]

**Output:** false

**Explanation:** The first player always plays "X".

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/05/15/tictactoe2-grid.jpg)

**Input:** board = ["XOX"," X "," "]

**Output:** false

**Explanation:** Players take turns making moves.

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/05/15/tictactoe4-grid.jpg)

**Input:** board = ["XOX","O O","XOX"]

**Output:** true

**Constraints:**

*   `board.length == 3`
*   `board[i].length == 3`
*   `board[i][j]` is either `'X'`, `'O'`, or `' '`.

## Solution

```kotlin
import kotlin.math.abs

class Solution {
    fun validTicTacToe(board: Array<String>): Boolean {
        // X=1,O=-1,’ ’=0
        var sum = 0
        val winsCol = IntArray(3)
        val winsDig = IntArray(2)
        var xWin = false
        var oWin = false
        for (i in 0..2) {
            val str = board[i]
            var rowSum = 0
            for (j in 0..2) {
                // char chr=str.toCharArray()[j];
                var intchr = 0
                if (str.toCharArray()[j] == 'X') {
                    intchr = 1
                }
                if (str.toCharArray()[j] == 'O') {
                    intchr = -1
                }
                rowSum += intchr
                winsCol[j] += intchr
                if (i == 2 && winsCol[j] == 3) {
                    xWin = true
                }
                if (i == 2 && winsCol[j] == -3) {
                    oWin = true
                }
                if (abs(i - j) != 1) {
                    if (i == j && i == 1) {
                        winsDig[0] += intchr
                        winsDig[1] += intchr
                    } else if (i == j) {
                        winsDig[0] += intchr
                    } else {
                        winsDig[1] += intchr
                    }
                }
                if (i == 2 && winsDig[0].coerceAtLeast(winsDig[1]) == 3) {
                    xWin = true
                }
                if (i == 2 && winsDig[0].coerceAtMost(winsDig[1]) == -3) {
                    oWin = true
                }
            }
            if (rowSum == 3) {
                xWin = true
            }
            if (rowSum == -3) {
                oWin = true
            }
            sum += rowSum
        }
        return if (sum == 0 && !xWin) {
            true
        } else sum == 1 && !oWin
    }
}
```