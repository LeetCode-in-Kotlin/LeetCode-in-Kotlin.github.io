[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2267\. Check if There Is a Valid Parentheses String Path

Hard

A parentheses string is a **non-empty** string consisting only of `'('` and `')'`. It is **valid** if **any** of the following conditions is **true**:

*   It is `()`.
*   It can be written as `AB` (`A` concatenated with `B`), where `A` and `B` are valid parentheses strings.
*   It can be written as `(A)`, where `A` is a valid parentheses string.

You are given an `m x n` matrix of parentheses `grid`. A **valid parentheses string path** in the grid is a path satisfying **all** of the following conditions:

*   The path starts from the upper left cell `(0, 0)`.
*   The path ends at the bottom-right cell `(m - 1, n - 1)`.
*   The path only ever moves **down** or **right**.
*   The resulting parentheses string formed by the path is **valid**.

Return `true` _if there exists a **valid parentheses string path** in the grid._ Otherwise, return `false`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/03/15/example1drawio.png)

**Input:** grid = \[\["(","(","("],[")","(",")"],["(","(",")"],["(","(",")"]]

**Output:** true

**Explanation:** The above diagram shows two possible paths that form valid parentheses strings. 

The first path shown results in the valid parentheses string "()(())". 

The second path shown results in the valid parentheses string "((()))". 

Note that there may be other valid parentheses string paths.

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/03/15/example2drawio.png)

**Input:** grid = \[\[")",")"],["(","("]]

**Output:** false

**Explanation:** The two possible paths form the parentheses strings "))(" and ")((". Since neither of them are valid parentheses strings, we return false.

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   `1 <= m, n <= 100`
*   `grid[i][j]` is either `'('` or `')'`.

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    private lateinit var grid: Array<CharArray>
    private var m = 0
    private var n = 0

    fun hasValidPath(grid: Array<CharArray>): Boolean {
        this.grid = grid
        m = grid.size
        n = grid[0].size
        val dp = Array(m) { Array(n) { arrayOfNulls<Boolean>(m + n + 1) } }
        if (grid[0][0] == RGTPAR) {
            return false
        }
        return if ((m + n) % 2 == 0) {
            false
        } else dfs(0, 0, 0, 0, dp)
    }

    private fun dfs(u: Int, v: Int, open: Int, close: Int, dp: Array<Array<Array<Boolean?>>>): Boolean {
        var open = open
        var close = close
        if (grid[u][v] == LFTPAR) {
            open++
        } else {
            close++
        }
        if (u == m - 1 && v == n - 1) {
            return open == close
        }
        if (open < close) {
            return false
        }
        if (dp[u][v][open - close] != null) {
            return dp[u][v][open - close]!!
        }
        if (u == m - 1) {
            val result = dfs(u, v + 1, open, close, dp)
            dp[u][v][open - close] = result
            return result
        }
        if (v == n - 1) {
            return dfs(u + 1, v, open, close, dp)
        }
        val rslt: Boolean
        rslt =
            if (grid[u][v] == LFTPAR) {
                dfs(u + 1, v, open, close, dp) || dfs(u, v + 1, open, close, dp)
            } else {
                dfs(u, v + 1, open, close, dp) || dfs(u + 1, v, open, close, dp)
            }
        dp[u][v][open - close] = rslt
        return rslt
    }

    companion object {
        private const val LFTPAR = '('
        private const val RGTPAR = ')'
    }
}
```