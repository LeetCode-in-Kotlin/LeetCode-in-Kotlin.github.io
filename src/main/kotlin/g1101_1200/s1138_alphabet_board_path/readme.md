[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1138\. Alphabet Board Path

Medium

On an alphabet board, we start at position `(0, 0)`, corresponding to character `board[0][0]`.

Here, `board = ["abcde", "fghij", "klmno", "pqrst", "uvwxy", "z"]`, as shown in the diagram below.

![](https://assets.leetcode.com/uploads/2019/07/28/azboard.png)

We may make the following moves:

*   `'U'` moves our position up one row, if the position exists on the board;
*   `'D'` moves our position down one row, if the position exists on the board;
*   `'L'` moves our position left one column, if the position exists on the board;
*   `'R'` moves our position right one column, if the position exists on the board;
*   `'!'` adds the character `board[r][c]` at our current position `(r, c)` to the answer.

(Here, the only positions that exist on the board are positions with letters on them.)

Return a sequence of moves that makes our answer equal to `target` in the minimum number of moves. You may return any path that does so.

**Example 1:**

**Input:** target = "leet"

**Output:** "DDR!UURRR!!DDD!"

**Example 2:**

**Input:** target = "code"

**Output:** "RR!DDRR!UUL!R!"

**Constraints:**

*   `1 <= target.length <= 100`
*   `target` consists only of English lowercase letters.

## Solution

```kotlin
class Solution {
    fun alphabetBoardPath(target: String): String {
        if (target.isEmpty()) {
            return ""
        }
        var sourceRow = 0
        var sourceCol = 0
        val path = StringBuilder()
        for (c in target.toCharArray()) {
            val position = c.code - 97
            val targetRow = position / 5
            val targetCol = position % 5
            if (targetCol < sourceCol) {
                path.append(helper("L", sourceCol - targetCol))
            }
            if (targetRow < sourceRow) {
                path.append(helper("U", sourceRow - targetRow))
            }
            if (targetRow > sourceRow) {
                path.append(helper("D", targetRow - sourceRow))
            }
            if (targetCol > sourceCol) {
                path.append(helper("R", targetCol - sourceCol))
            }
            path.append("!")
            sourceRow = targetRow
            sourceCol = targetCol
        }
        return path.toString()
    }

    fun helper(dir: String?, time: Int): StringBuilder {
        val path = StringBuilder()
        for (i in 0 until time) {
            path.append(dir)
        }
        return path
    }
}
```