[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 488\. Zuma Game

Hard

You are playing a variation of the game Zuma.

In this variation of Zuma, there is a **single row** of colored balls on a board, where each ball can be colored red `'R'`, yellow `'Y'`, blue `'B'`, green `'G'`, or white `'W'`. You also have several colored balls in your hand.

Your goal is to **clear all** of the balls from the board. On each turn:

*   Pick **any** ball from your hand and insert it in between two balls in the row or on either end of the row.
*   If there is a group of **three or more consecutive balls** of the **same color**, remove the group of balls from the board.
    *   If this removal causes more groups of three or more of the same color to form, then continue removing each group until there are none left.
*   If there are no more balls on the board, then you win the game.
*   Repeat this process until you either win or do not have any more balls in your hand.

Given a string `board`, representing the row of balls on the board, and a string `hand`, representing the balls in your hand, return _the **minimum** number of balls you have to insert to clear all the balls from the board. If you cannot clear all the balls from the board using the balls in your hand, return_ `-1`.

**Example 1:**

**Input:** board = "WRRBBW", hand = "RB"

**Output:** -1

**Explanation:** It is impossible to clear all the balls. The best you can do is: - Insert 'R' so the board becomes WRR<ins>R</ins>BBW. W<ins>RRR</ins>BBW -> WBBW. - Insert 'B' so the board becomes WBB<ins>B</ins>W. W<ins>BBB</ins>W -> WW. There are still balls remaining on the board, and you are out of balls to insert.

**Example 2:**

**Input:** board = "WWRRBBWW", hand = "WRBRW"

**Output:** 2

**Explanation:** To make the board empty: - Insert 'R' so the board becomes WWRR<ins>R</ins>BBWW. WW<ins>RRR</ins>BBWW -> WWBBWW. - Insert 'B' so the board becomes WWBB<ins>B</ins>WW. WW<ins>BBB</ins>WW -> <ins>WWWW</ins> -> empty. 2 balls from your hand were needed to clear the board.

**Example 3:**

**Input:** board = "G", hand = "GGGGG"

**Output:** 2

**Explanation:** To make the board empty: - Insert 'G' so the board becomes G<ins>G</ins>. - Insert 'G' so the board becomes GG<ins>G</ins>. <ins>GGG</ins> -> empty. 2 balls from your hand were needed to clear the board.

**Constraints:**

*   `1 <= board.length <= 16`
*   `1 <= hand.length <= 5`
*   `board` and `hand` consist of the characters `'R'`, `'Y'`, `'B'`, `'G'`, and `'W'`.
*   The initial row of balls on the board will **not** have any groups of three or more consecutive balls of the same color.

## Solution

```kotlin
class Solution {
    fun findMinStep(board: String, hand: String): Int {
        return dfs(board, hand)
    }

    private fun dfs(board: String, hand: String): Int {
        return findMinStepDp(board, hand, HashMap())
    }

    private fun findMinStepDp(board: String, hand: String, dp: MutableMap<String, MutableMap<String, Int?>?>): Int {
        if (board.isEmpty()) {
            return 0
        }
        if (hand.isEmpty()) {
            return -1
        }
        if (dp[board] != null && dp[board]!![hand] != null) {
            return dp[board]!![hand]!!
        }
        var min = -1
        for (i in 0..board.length) {
            for (j in 0 until hand.length) {
                if ((j == 0 || hand[j] != hand[j - 1]) &&
                    (i == 0 || board[i - 1] != hand[j]) &&
                    (
                        i < board.length && board[i] == hand[j] || i > 0 &&
                            i < board.length && board[i - 1] == board[i] && board[i] != hand[j]
                        )
                ) {
                    val newS = StringBuilder(board)
                    newS.insert(i, hand[j])
                    val sR = findMinStepDp(
                        removeRepeated(newS.toString()),
                        hand.substring(0, j) + hand.substring(j + 1, hand.length),
                        dp,
                    )
                    if (sR != -1) {
                        min = if (min == -1) sR + 1 else Integer.min(min, sR + 1)
                    }
                }
            }
        }
        dp.putIfAbsent(board, HashMap())
        dp[board]!![hand] = min
        return min
    }

    private fun removeRepeated(original: String): String {
        var count = 1
        var i = 1
        while (i < original.length) {
            if (original[i] == original[i - 1]) {
                count++
                i++
            } else {
                if (count >= 3) {
                    return removeRepeated(
                        original.substring(0, i - count) +
                            original.substring(i, original.length),
                    )
                } else {
                    count = 1
                    i++
                }
            }
        }
        return if (count >= 3) {
            removeRepeated(original.substring(0, original.length - count))
        } else {
            original
        }
    }
}
```