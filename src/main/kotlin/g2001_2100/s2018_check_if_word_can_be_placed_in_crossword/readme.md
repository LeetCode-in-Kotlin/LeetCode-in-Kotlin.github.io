[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2018\. Check if Word Can Be Placed In Crossword

Medium

You are given an `m x n` matrix `board`, representing the **current** state of a crossword puzzle. The crossword contains lowercase English letters (from solved words), `' '` to represent any **empty** cells, and `'#'` to represent any **blocked** cells.

A word can be placed **horizontally** (left to right **or** right to left) or **vertically** (top to bottom **or** bottom to top) in the board if:

*   It does not occupy a cell containing the character `'#'`.
*   The cell each letter is placed in must either be `' '` (empty) or **match** the letter already on the `board`.
*   There must not be any empty cells `' '` or other lowercase letters **directly left or right** of the word if the word was placed **horizontally**.
*   There must not be any empty cells `' '` or other lowercase letters **directly above or below** the word if the word was placed **vertically**.

Given a string `word`, return `true` _if_ `word` _can be placed in_ `board`_, or_ `false` _**otherwise**_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/10/04/crossword-ex1-1.png)

**Input:** board = \[\["#", " ", "#"], [" ", " ", "#"], ["#", "c", " "]], word = "abc"

**Output:** true

**Explanation:** The word "abc" can be placed as shown above (top to bottom).

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/10/04/crossword-ex2-1.png)

**Input:** board = \[\[" ", "#", "a"], [" ", "#", "c"], [" ", "#", "a"]], word = "ac"

**Output:** false

**Explanation:** It is impossible to place the word because there will always be a space/letter above or below it.

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/10/04/crossword-ex3-1.png)

**Input:** board = \[\["#", " ", "#"], [" ", " ", "#"], ["#", " ", "c"]], word = "ca"

**Output:** true

**Explanation:** The word "ca" can be placed as shown above (right to left).

**Constraints:**

*   `m == board.length`
*   `n == board[i].length`
*   <code>1 <= m * n <= 2 * 10<sup>5</sup></code>
*   `board[i][j]` will be `' '`, `'#'`, or a lowercase English letter.
*   `1 <= word.length <= max(m, n)`
*   `word` will contain only lowercase English letters.

## Solution

```kotlin
class Solution {
    fun placeWordInCrossword(board: Array<CharArray>, word: String): Boolean {
        val m = board.size
        val n = board[0].size
        for (i in 0 until m) {
            for (j in 0 until n) {
                if ((board[i][j] == ' ' || board[i][j] == word[0]) &&
                    (
                        canPlaceTopDown(word, board, i, j) ||
                            canPlaceLeftRight(word, board, i, j) ||
                            canPlaceBottomUp(word, board, i, j) ||
                            canPlaceRightLeft(word, board, i, j)
                        )
                ) {
                    return true
                }
            }
        }
        return false
    }

    private fun canPlaceRightLeft(word: String, board: Array<CharArray>, row: Int, col: Int): Boolean {
        if (col + 1 < board[0].size &&
            (Character.isLowerCase(board[row][col + 1]) || board[row][col + 1] == ' ')
        ) {
            return false
        }
        var k = 0
        var j = col
        while (j >= 0 && k < word.length) {
            if (board[row][j] != word[k] && board[row][j] != ' ') {
                return false
            } else {
                k++
            }
            j--
        }
        return k == word.length && (j < 0 || board[row][j] == '#')
    }

    private fun canPlaceBottomUp(word: String, board: Array<CharArray>, row: Int, col: Int): Boolean {
        if (row + 1 < board.size &&
            (Character.isLowerCase(board[row + 1][col]) || board[row + 1][col] == ' ')
        ) {
            return false
        }
        var k = 0
        var i = row
        while (i >= 0 && k < word.length) {
            if (board[i][col] != word[k] && board[i][col] != ' ') {
                return false
            } else {
                k++
            }
            i--
        }
        return k == word.length && (i < 0 || board[i][col] == '#')
    }

    private fun canPlaceLeftRight(word: String, board: Array<CharArray>, row: Int, col: Int): Boolean {
        if (col > 0 && (Character.isLowerCase(board[row][col - 1]) || board[row][col - 1] == ' ')) {
            return false
        }
        var k = 0
        var j = col
        while (j < board[0].size && k < word.length) {
            if (board[row][j] != word[k] && board[row][j] != ' ') {
                return false
            } else {
                k++
            }
            j++
        }
        return k == word.length && (j == board[0].size || board[row][j] == '#')
    }

    private fun canPlaceTopDown(word: String, board: Array<CharArray>, row: Int, col: Int): Boolean {
        if (row > 0 && (Character.isLowerCase(board[row - 1][col]) || board[row - 1][col] == ' ')) {
            return false
        }
        var k = 0
        var i = row
        while (i < board.size && k < word.length) {
            if (board[i][col] != word[k] && board[i][col] != ' ') {
                return false
            } else {
                k++
            }
            i++
        }
        return k == word.length && (i == board.size || board[i][col] == '#')
    }
}
```