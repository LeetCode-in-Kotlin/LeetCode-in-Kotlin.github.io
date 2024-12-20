[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1406\. Stone Game III

Hard

Alice and Bob continue their games with piles of stones. There are several stones **arranged in a row**, and each stone has an associated value which is an integer given in the array `stoneValue`.

Alice and Bob take turns, with Alice starting first. On each player's turn, that player can take `1`, `2`, or `3` stones from the **first** remaining stones in the row.

The score of each player is the sum of the values of the stones taken. The score of each player is `0` initially.

The objective of the game is to end with the highest score, and the winner is the player with the highest score and there could be a tie. The game continues until all the stones have been taken.

Assume Alice and Bob **play optimally**.

Return `"Alice"` _if Alice will win,_ `"Bob"` _if Bob will win, or_ `"Tie"` _if they will end the game with the same score_.

**Example 1:**

**Input:** values = [1,2,3,7]

**Output:** "Bob"

**Explanation:** Alice will always lose. Her best move will be to take three piles and the score become 6. Now the score of Bob is 7 and Bob wins.

**Example 2:**

**Input:** values = [1,2,3,-9]

**Output:** "Alice"

**Explanation:** Alice must choose all the three piles at the first move to win and leave Bob with negative score. If Alice chooses one pile her score will be 1 and the next move Bob's score becomes 5. In the next move, Alice will take the pile with value = -9 and lose. If Alice chooses two piles her score will be 3 and the next move Bob's score becomes 3. In the next move, Alice will take the pile with value = -9 and also lose. Remember that both play optimally so here Alice will choose the scenario that makes her win.

**Example 3:**

**Input:** values = [1,2,3,6]

**Output:** "Tie"

**Explanation:** Alice cannot win this game. She can end the game in a draw if she decided to choose all the first three piles, otherwise she will lose.

**Constraints:**

*   <code>1 <= stoneValue.length <= 5 * 10<sup>4</sup></code>
*   `-1000 <= stoneValue[i] <= 1000`

## Solution

```kotlin
class Solution {
    fun stoneGameIII(stoneValue: IntArray): String {
        val dp = IntArray(stoneValue.size + 1)
        dp.fill(0)
        var i = stoneValue.size - 1
        while (i >= 0) {
            var ans = Int.MIN_VALUE
            ans = Math.max(ans, stoneValue[i] - dp[i + 1])
            if (i + 1 < stoneValue.size) {
                ans = Math.max(ans, stoneValue[i] + stoneValue[i + 1] - dp[i + 2])
            }
            if (i + 2 < stoneValue.size) {
                ans = Math.max(
                    ans,
                    stoneValue[i] + stoneValue[i + 1] + stoneValue[i + 2] - dp[i + 3],
                )
            }
            dp[i] = ans
            i--
        }
        val value = dp[0]
        if (value > 0) {
            return "Alice"
        }
        return if (value == 0) "Tie" else "Bob"
    }
}
```