[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1434\. Number of Ways to Wear Different Hats to Each Other

Hard

There are `n` people and `40` types of hats labeled from `1` to `40`.

Given a 2D integer array `hats`, where `hats[i]` is a list of all hats preferred by the <code>i<sup>th</sup></code> person.

Return _the number of ways that the `n` people wear different hats to each other_.

Since the answer may be too large, return it modulo <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** hats = \[\[3,4],[4,5],[5]]

**Output:** 1

**Explanation:** There is only one way to choose hats given the conditions. First person choose hat 3, Second person choose hat 4 and last one hat 5.

**Example 2:**

**Input:** hats = \[\[3,5,1],[3,5]]

**Output:** 4

**Explanation:** There are 4 ways to choose hats: (3,5), (5,3), (1,3) and (1,5)

**Example 3:**

**Input:** hats = \[\[1,2,3,4],[1,2,3,4],[1,2,3,4],[1,2,3,4]]

**Output:** 24

**Explanation:** Each person can choose hats labeled from 1 to 4. Number of Permutations of (1,2,3,4) = 24.

**Constraints:**

*   `n == hats.length`
*   `1 <= n <= 10`
*   `1 <= hats[i].length <= 40`
*   `1 <= hats[i][j] <= 40`
*   `hats[i]` contains a list of **unique** integers.

## Solution

```kotlin
class Solution {
    fun numberWays(hats: List<List<Int>>): Int {
        val mod = 1000000007L
        val size = hats.size
        val possible = Array(size) { BooleanArray(41) }
        for (i in 0 until size) {
            for (j in hats[i]) {
                possible[i][j] = true
            }
        }
        val dp = LongArray(1 shl size)
        dp[0] = 1
        for (i in 1..40) {
            for (j in dp.size - 1 downTo 1) {
                for (k in 0 until size) {
                    if (j shr k and 1 == 1 && possible[k][i]) {
                        dp[j] += dp[j xor (1 shl k)]
                    }
                }
                dp[j] %= mod
            }
        }
        return dp[(1 shl size) - 1].toInt()
    }
}
```