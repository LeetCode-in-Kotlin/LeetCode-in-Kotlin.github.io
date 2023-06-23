[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1958\. Check if Move is Legal

Medium

You are given a **0-indexed** `8 x 8` grid `board`, where `board[r][c]` represents the cell `(r, c)` on a game board. On the board, free cells are represented by `'.'`, white cells are represented by `'W'`, and black cells are represented by `'B'`.

Each move in this game consists of choosing a free cell and changing it to the color you are playing as (either white or black). However, a move is only **legal** if, after changing it, the cell becomes the **endpoint of a good line** (horizontal, vertical, or diagonal).

A **good line** is a line of **three or more cells (including the endpoints)** where the endpoints of the line are **one color**, and the remaining cells in the middle are the **opposite color** (no cells in the line are free). You can find examples for good lines in the figure below:

![](https://assets.leetcode.com/uploads/2021/07/22/goodlines5.png)

Given two integers `rMove` and `cMove` and a character `color` representing the color you are playing as (white or black), return `true` _if changing cell_ `(rMove, cMove)` _to color_ `color` _is a **legal** move, or_ `false` _if it is not legal_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/07/10/grid11.png)

**Input:**

    board = [ [".",".",".","B",".",".",".","."],
             [".",".",".","W",".",".",".","."],
             [".",".",".","W",".",".",".","."],
             [".",".",".","W",".",".",".","."],
             ["W","B","B",".","W","W","W","B"],
             [".",".",".","B",".",".",".","."],
             [".",".",".","B",".",".",".","."],
             [".",".",".","W",".",".",".","."]],
    rMove = 4, cMove = 3, color = "B"

**Output:** true

**Explanation:** '.', 'W', and 'B' are represented by the colors blue, white, and black respectively, and cell (rMove, cMove) is marked with an 'X'. The two good lines with the chosen cell as an endpoint are annotated above with the red rectangles.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/07/10/grid2.png)

**Input:**

    board = [ [".",".",".",".",".",".",".","."],
             [".","B",".",".","W",".",".","."],
             [".",".","W",".",".",".",".","."],
             [".",".",".","W","B",".",".","."],
             [".",".",".",".",".",".",".","."],
             [".",".",".",".","B","W",".","."],
             [".",".",".",".",".",".","W","."],
             [".",".",".",".",".",".",".","B"]],
    rMove = 4, cMove = 4, color = "W"

**Output:** false

**Explanation:** While there are good lines with the chosen cell as a middle cell, there are no good lines with the chosen cell as an endpoint.

**Constraints:**

*   `board.length == board[r].length == 8`
*   `0 <= rMove, cMove < 8`
*   `board[rMove][cMove] == '.'`
*   `color` is either `'B'` or `'W'`.

## Solution

```kotlin
class Solution {
    fun checkMove(board: Array<CharArray>, rMove: Int, cMove: Int, color: Char): Boolean {
        val opposite = if (color == 'W') 'B' else if (color == 'B') 'W' else ' '
        if (opposite == ' ' || !find(board, rMove, cMove, '.')) {
            return false
        }
        for (dir in DIRS) {
            var rNext = rMove + dir[0]
            var cNext = cMove + dir[1]
            if (find(board, rNext, cNext, opposite)) {
                rNext += dir[0]
                cNext += dir[1]
                while (find(board, rNext, cNext, opposite)) {
                    rNext += dir[0]
                    cNext += dir[1]
                }
                if (find(board, rNext, cNext, color)) {
                    return true
                }
            }
        }
        return false
    }

    private fun find(board: Array<CharArray>, r: Int, c: Int, target: Char): Boolean {
        return r >= 0 && r < board.size && c >= 0 && c < board[r].size && board[r][c] == target
    }

    companion object {
        private val DIRS = arrayOf(
            intArrayOf(-1, -1),
            intArrayOf(-1, 0),
            intArrayOf(-1, 1),
            intArrayOf(0, -1),
            intArrayOf(0, 1),
            intArrayOf(1, -1),
            intArrayOf(1, 0),
            intArrayOf(1, 1)
        )
    }
}
```