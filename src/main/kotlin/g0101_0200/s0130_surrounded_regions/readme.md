[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 130\. Surrounded Regions

Medium

Given an `m x n` matrix `board` containing `'X'` and `'O'`, _capture all regions that are 4-directionally surrounded by_ `'X'`.

A region is **captured** by flipping all `'O'`s into `'X'`s in that surrounded region.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/xogrid.jpg)

**Input:** board = \[\["X","X","X","X"],["X","O","O","X"],["X","X","O","X"],["X","O","X","X"]]

**Output:** [["X","X","X","X"],["X","X","X","X"],["X","X","X","X"],["X","O","X","X"]]

**Explanation:** Notice that an 'O' should not be flipped if:

- It is on the border, or

- It is adjacent to an 'O' that should not be flipped. 

The bottom 'O' is on the border, so it is not flipped. 
 
The other three 'O' form a surrounded region, so they are flipped.

**Example 2:**

**Input:** board = \[\["X"]]

**Output:** [["X"]]

**Constraints:**

*   `m == board.length`
*   `n == board[i].length`
*   `1 <= m, n <= 200`
*   `board[i][j]` is `'X'` or `'O'`.

## Solution

```kotlin
class Solution {
    fun solve(board: Array<CharArray>) {
        // Edge case, empty grid
        if (board.size == 0) {
            return
        }
        // Traverse first and last rows ( boundaries)
        for (i in board[0].indices) {
            // first row
            if (board[0][i] == 'O') {
                // It will covert O and all it's touching O's to #
                dfs(board, 0, i)
            }
            // last row
            if (board[board.size - 1][i] == 'O') {
                // Coverts O's to #'s (same thing as above)
                dfs(board, board.size - 1, i)
            }
        }
        // Traverse first and last Column (boundaries)
        for (i in board.indices) {
            // first Column
            if (board[i][0] == 'O') {
                // Converts O's to #'s
                dfs(board, i, 0)
            }
            // last Column
            if (board[i][board[0].size - 1] == 'O') {
                // Coverts O's to #'s
                dfs(board, i, board[0].size - 1)
            }
        }
        // Traverse through entire matrix
        for (i in board.indices) {
            for (j in board[0].indices) {
                if (board[i][j] == 'O') {
                    // Convert O's to X's
                    board[i][j] = 'X'
                }
                if (board[i][j] == '#') {
                    // Convert #'s to O's
                    board[i][j] = 'O'
                }
            }
        }
    }

    fun dfs(board: Array<CharArray>, row: Int, column: Int) {
        if (row < 0 || row >= board.size || column < 0 || column >= board[0].size || board[row][column] != 'O') {
            return
        }
        board[row][column] = '#'
        dfs(board, row + 1, column)
        dfs(board, row - 1, column)
        dfs(board, row, column + 1)
        dfs(board, row, column - 1)
    }
}
```