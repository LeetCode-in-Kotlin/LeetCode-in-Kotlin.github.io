[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1563\. Stone Game V

Hard

There are several stones **arranged in a row**, and each stone has an associated value which is an integer given in the array `stoneValue`.

In each round of the game, Alice divides the row into **two non-empty rows** (i.e. left row and right row), then Bob calculates the value of each row which is the sum of the values of all the stones in this row. Bob throws away the row which has the maximum value, and Alice's score increases by the value of the remaining row. If the value of the two rows are equal, Bob lets Alice decide which row will be thrown away. The next round starts with the remaining row.

The game ends when there is only **one stone remaining**. Alice's is initially **zero**.

Return _the maximum score that Alice can obtain_.

**Example 1:**

**Input:** stoneValue = [6,2,3,4,5,5]

**Output:** 18

**Explanation:** In the first round, Alice divides the row to [6,2,3], [4,5,5]. The left row has the value 11 and the right row has value 14. Bob throws away the right row and Alice's score is now 11. In the second round Alice divides the row to [6], [2,3]. This time Bob throws away the left row and Alice's score becomes 16 (11 + 5).

    The last round Alice has only one choice to divide the row which is [2], [3]. Bob throws away the right row and Alice's score is now 18 (16 + 2). The game ends because only one stone is remaining in the row.

**Example 2:**

**Input:** stoneValue = [7,7,7,7,7,7,7]

**Output:** 28

**Example 3:**

**Input:** stoneValue = [4]

**Output:** 0

**Constraints:**

*   `1 <= stoneValue.length <= 500`
*   <code>1 <= stoneValue[i] <= 10<sup>6</sup></code>

## Solution

```kotlin
class Solution {
    fun stoneGameV(stoneValue: IntArray): Int {
        val n = stoneValue.size
        val ps = IntArray(n)
        ps[0] = stoneValue[0]
        for (i in 1 until n) {
            ps[i] = ps[i - 1] + stoneValue[i]
        }
        return gameDP(ps, 0, n - 1, Array<Array<Int?>>(n) { arrayOfNulls(n) })
    }

    private fun gameDP(ps: IntArray, i: Int, j: Int, dp: Array<Array<Int?>>): Int {
        if (i == j) {
            return 0
        }
        if (dp[i][j] != null) {
            return dp[i][j]!!
        }
        var max = 0
        for (k in i + 1..j) {
            val l = ps[k - 1] - if (i == 0) 0 else ps[i - 1]
            val r = ps[j] - ps[k - 1]
            if (2 * Math.min(l, r) < max) {
                continue
            }
            if (l <= r) {
                max = Math.max(max, l + gameDP(ps, i, k - 1, dp))
            }
            if (l >= r) {
                max = Math.max(max, r + gameDP(ps, k, j, dp))
            }
        }
        dp[i][j] = max
        return max
    }
}
```