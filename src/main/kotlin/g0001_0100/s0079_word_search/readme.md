[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 79\. Word Search

Medium

Given an `m x n` grid of characters `board` and a string `word`, return `true` _if_ `word` _exists in the grid_.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/04/word2.jpg)

**Input:** board = \[\["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"

**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/04/word-1.jpg)

**Input:** board = \[\["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "SEE"

**Output:** true

**Example 3:**

![](https://assets.leetcode.com/uploads/2020/10/15/word3.jpg)

**Input:** board = \[\["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCB"

**Output:** false

**Constraints:**

*   `m == board.length`
*   `n = board[i].length`
*   `1 <= m, n <= 6`
*   `1 <= word.length <= 15`
*   `board` and `word` consists of only lowercase and uppercase English letters.

**Follow up:** Could you use search pruning to make your solution faster with a larger `board`?

## Solution

```kotlin
class Solution {
    private fun backtrace(
        board: Array<CharArray>,
        visited: Array<BooleanArray>,
        word: String,
        index: Int,
        x: Int,
        y: Int
    ): Boolean {
        if (index == word.length) {
            return true
        }
        if (x < 0 || x >= board.size || y < 0 || y >= board[0].size || visited[x][y]) {
            return false
        }
        visited[x][y] = true
        return if (word[index] == board[x][y]) {
            val res = (
                backtrace(board, visited, word, index + 1, x, y + 1) ||
                    backtrace(board, visited, word, index + 1, x, y - 1) ||
                    backtrace(board, visited, word, index + 1, x + 1, y) ||
                    backtrace(board, visited, word, index + 1, x - 1, y)
                )
            if (!res) {
                visited[x][y] = false
            }
            res
        } else {
            visited[x][y] = false
            false
        }
    }

    fun exist(board: Array<CharArray>, word: String): Boolean {
        val visited = Array(board.size) {
            BooleanArray(
                board[0].size
            )
        }
        for (i in board.indices) {
            for (j in board[0].indices) {
                if (backtrace(board, visited, word, 0, i, j)) {
                    return true
                }
            }
        }
        return false
    }
}
```