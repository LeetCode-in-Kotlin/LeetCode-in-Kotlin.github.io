[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 289\. Game of Life

Medium

According to [Wikipedia's article](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life): "The **Game of Life**, also known simply as **Life**, is a cellular automaton devised by the British mathematician John Horton Conway in 1970."

The board is made up of an `m x n` grid of cells, where each cell has an initial state: **live** (represented by a `1`) or **dead** (represented by a `0`). Each cell interacts with its [eight neighbors](https://en.wikipedia.org/wiki/Moore_neighborhood) (horizontal, vertical, diagonal) using the following four rules (taken from the above Wikipedia article):

1.  Any live cell with fewer than two live neighbors dies as if caused by under-population.
2.  Any live cell with two or three live neighbors lives on to the next generation.
3.  Any live cell with more than three live neighbors dies, as if by over-population.
4.  Any dead cell with exactly three live neighbors becomes a live cell, as if by reproduction.

The next state is created by applying the above rules simultaneously to every cell in the current state, where births and deaths occur simultaneously. Given the current state of the `m x n` grid `board`, return _the next state_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/12/26/grid1.jpg)

**Input:** board = \[\[0,1,0],[0,0,1],[1,1,1],[0,0,0]]

**Output:** [[0,0,0],[1,0,1],[0,1,1],[0,1,0]]

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/12/26/grid2.jpg)

**Input:** board = \[\[1,1],[1,0]]

**Output:** [[1,1],[1,1]]

**Constraints:**

*   `m == board.length`
*   `n == board[i].length`
*   `1 <= m, n <= 25`
*   `board[i][j]` is `0` or `1`.

**Follow up:**

*   Could you solve it in-place? Remember that the board needs to be updated simultaneously: You cannot update some cells first and then use their updated values to update other cells.
*   In this question, we represent the board using a 2D array. In principle, the board is infinite, which would cause problems when the active area encroaches upon the border of the array (i.e., live cells reach the border). How would you address these problems?

## Solution

```kotlin
class Solution {
    companion object {
        var dim: Array<IntArray> = arrayOf(
            intArrayOf(1, 0), intArrayOf(0, 1), intArrayOf(-1, 0), intArrayOf(0, -1),
            intArrayOf(1, 1), intArrayOf(1, -1), intArrayOf(-1, 1), intArrayOf(-1, -1)
        )
    }

    fun gameOfLife(board: Array<IntArray>) {
        for (i in board.indices) {
            for (j in board[0].indices) {
                val com = compute(board, i, j)
                if (board[i][j] == 0) {
                    if (com == 3) {
                        board[i][j] = -1
                    }
                } else if (board[i][j] == 1 && (com < 2 || com > 3)) {
                    board[i][j] = 2
                }
            }
        }

        update(board)
    }

    private fun update(board: Array<IntArray>) {
        for (i in board.indices) {
            for (j in board[0].indices) {
                if (board[i][j] == -1) {
                    board[i][j] = 1
                } else if (board[i][j] == 2) {
                    board[i][j] = 0
                }
            }
        }
    }

    private fun compute(board: Array<IntArray>, r: Int, c: Int): Int {
        var ret: Int = 0
        for (arr in dim) {
            val row = arr[0] + r
            val col = arr[1] + c
            if (row >= 0 && row < board.size && col >= 0 && col < board[0].size &&
                (board[row][col] == 1 || board[row][col] == 2)
            ) {
                ret++
            }
        }
        return ret
    }
}
```