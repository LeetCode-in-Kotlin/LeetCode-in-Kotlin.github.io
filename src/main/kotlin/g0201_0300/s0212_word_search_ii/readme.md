[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 212\. Word Search II

Hard

Given an `m x n` `board` of characters and a list of strings `words`, return _all words on the board_.

Each word must be constructed from letters of sequentially adjacent cells, where **adjacent cells** are horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/07/search1.jpg)

**Input:** board = \[\["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]], words = ["oath","pea","eat","rain"]

**Output:** ["eat","oath"]

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/07/search2.jpg)

**Input:** board = \[\["a","b"],["c","d"]], words = ["abcb"]

**Output:** []

**Constraints:**

*   `m == board.length`
*   `n == board[i].length`
*   `1 <= m, n <= 12`
*   `board[i][j]` is a lowercase English letter.
*   <code>1 <= words.length <= 3 * 10<sup>4</sup></code>
*   `1 <= words[i].length <= 10`
*   `words[i]` consists of lowercase English letters.
*   All the strings of `words` are unique.

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    private var root: Tree? = null
    fun findWords(board: Array<CharArray>, words: Array<String?>): List<String> {
        if (board.size < 1 || board[0].size < 1) {
            return emptyList()
        }
        root = Tree()
        for (word in words) {
            Tree.addWord(root, word!!)
        }
        val collected: MutableList<String> = ArrayList()
        for (i in board.indices) {
            for (j in board[0].indices) {
                dfs(board, i, j, root, collected)
            }
        }
        return collected
    }

    private fun dfs(board: Array<CharArray>, i: Int, j: Int, cur: Tree?, collected: MutableList<String>) {
        var cur: Tree? = cur
        val c = board[i][j]
        if (c == '-') {
            return
        }
        cur = cur!!.getChild(c)
        if (cur == null) {
            return
        }
        if (cur.end != null) {
            val s: String = cur.end!!
            collected.add(s)
            cur.end = null
            if (cur.len() == 0) {
                Tree.deleteWord(root, s)
            }
        }
        board[i][j] = '-'
        if (i > 0) {
            dfs(board, i - 1, j, cur, collected)
        }
        if (i + 1 < board.size) {
            dfs(board, i + 1, j, cur, collected)
        }
        if (j > 0) {
            dfs(board, i, j - 1, cur, collected)
        }
        if (j + 1 < board[0].size) {
            dfs(board, i, j + 1, cur, collected)
        }
        board[i][j] = c
    }
}
```