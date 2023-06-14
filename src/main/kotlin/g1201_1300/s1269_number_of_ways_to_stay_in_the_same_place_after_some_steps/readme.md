[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1269\. Number of Ways to Stay in the Same Place After Some Steps

Hard

You have a pointer at index `0` in an array of size `arrLen`. At each step, you can move 1 position to the left, 1 position to the right in the array, or stay in the same place (The pointer should not be placed outside the array at any time).

Given two integers `steps` and `arrLen`, return the number of ways such that your pointer still at index `0` after **exactly** `steps` steps. Since the answer may be too large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** steps = 3, arrLen = 2

**Output:** 4

**Explanation:** There are 4 differents ways to stay at index 0 after 3 steps. 

Right, Left, Stay 

Stay, Right, Left 

Right, Stay, Left 

Stay, Stay, Stay

**Example 2:**

**Input:** steps = 2, arrLen = 4

**Output:** 2

**Explanation:** There are 2 differents ways to stay at index 0 after 2 steps 

Right, Left 

Stay, Stay

**Example 3:**

**Input:** steps = 4, arrLen = 2

**Output:** 8

**Constraints:**

*   `1 <= steps <= 500`
*   <code>1 <= arrLen <= 10<sup>6</sup></code>

## Solution

```kotlin
class Solution {
    private var n = 0
    private lateinit var dp: Array<IntArray>

    private fun dfs(i: Int, st: Int): Int {
        if (i < 0 || i >= n) {
            return 0
        }
        if (st == 0) {
            return if (i == 0) 1 else 0
        }
        if (dp[i][st] == -1) {
            val mod = 1000000007
            dp[i][st] = ((dfs(i + 1, st - 1) + dfs(i, st - 1)) % mod + dfs(i - 1, st - 1)) % mod
        }
        return dp[i][st]
    }

    fun numWays(steps: Int, arrLen: Int): Int {
        n = Math.min(steps, arrLen)
        dp = Array(n) { IntArray(steps + 1) }
        for (i in 0 until n) {
            dp[i].fill(-1)
        }
        dfs(0, steps)
        return dp[0][steps]
    }
}
```