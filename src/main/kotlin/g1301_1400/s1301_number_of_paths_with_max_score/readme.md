[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1301\. Number of Paths with Max Score

Hard

You are given a square `board` of characters. You can move on the board starting at the bottom right square marked with the character `'S'`.

You need to reach the top left square marked with the character `'E'`. The rest of the squares are labeled either with a numeric character `1, 2, ..., 9` or with an obstacle `'X'`. In one move you can go up, left or up-left (diagonally) only if there is no obstacle there.

Return a list of two integers: the first integer is the maximum sum of numeric characters you can collect, and the second is the number of such paths that you can take to get that maximum sum, **taken modulo `10^9 + 7`**.

In case there is no path, return `[0, 0]`.

**Example 1:**

**Input:** board = ["E23","2X2","12S"]

**Output:** [7,1]

**Example 2:**

**Input:** board = ["E12","1X1","21S"]

**Output:** [4,2]

**Example 3:**

**Input:** board = ["E11","XXX","11S"]

**Output:** [0,0]

**Constraints:**

*   `2 <= board.length == board[i].length <= 100`

## Solution

```kotlin
class Solution {
    fun pathsWithMaxScore(board: List<String>): IntArray {
        val rows = board.size
        val columns = board[0].length
        val dp = Array(rows) { Array(columns) { IntArray(2) } }
        for (r in rows - 1 downTo 0) {
            for (c in columns - 1 downTo 0) {
                val current = board[r][c]
                if (current == 'S') {
                    dp[r][c][0] = 0
                    dp[r][c][1] = 1
                } else if (current != 'X') {
                    var maxScore = 0
                    var paths = 0
                    val currentScore = if (current == 'E') 0 else current.code - '0'.code
                    for (dir in DIRECTIONS) {
                        val nextR = r + dir[0]
                        val nextC = c + dir[1]
                        if (nextR < rows && nextC < columns && dp[nextR][nextC][1] > 0) {
                            if (dp[nextR][nextC][0] + currentScore > maxScore) {
                                maxScore = dp[nextR][nextC][0] + currentScore
                                paths = dp[nextR][nextC][1]
                            } else if (dp[nextR][nextC][0] + currentScore == maxScore) {
                                paths = (paths + dp[nextR][nextC][1]) % 1000000007
                            }
                        }
                    }
                    dp[r][c][0] = maxScore
                    dp[r][c][1] = paths
                }
            }
        }
        return intArrayOf(dp[0][0][0], dp[0][0][1])
    }

    companion object {
        private val DIRECTIONS = arrayOf(intArrayOf(1, 0), intArrayOf(0, 1), intArrayOf(1, 1))
    }
}
```