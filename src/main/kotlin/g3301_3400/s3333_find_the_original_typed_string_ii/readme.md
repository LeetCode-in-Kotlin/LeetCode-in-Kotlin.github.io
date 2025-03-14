[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3333\. Find the Original Typed String II

Hard

Alice is attempting to type a specific string on her computer. However, she tends to be clumsy and **may** press a key for too long, resulting in a character being typed **multiple** times.

You are given a string `word`, which represents the **final** output displayed on Alice's screen. You are also given a **positive** integer `k`.

Return the total number of _possible_ original strings that Alice _might_ have intended to type, if she was trying to type a string of size **at least** `k`.

Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** word = "aabbccdd", k = 7

**Output:** 5

**Explanation:**

The possible strings are: `"aabbccdd"`, `"aabbccd"`, `"aabbcdd"`, `"aabccdd"`, and `"abbccdd"`.

**Example 2:**

**Input:** word = "aabbccdd", k = 8

**Output:** 1

**Explanation:**

The only possible string is `"aabbccdd"`.

**Example 3:**

**Input:** word = "aaabbb", k = 3

**Output:** 8

**Constraints:**

*   <code>1 <= word.length <= 5 * 10<sup>5</sup></code>
*   `word` consists only of lowercase English letters.
*   `1 <= k <= 2000`

## Solution

```kotlin
class Solution {
    fun possibleStringCount(word: String, k: Int): Int {
        val list: MutableList<Int> = ArrayList<Int>()
        val n = word.length
        var i = 0
        while (i < n) {
            var j = i + 1
            while (j < n && word[j] == word[j - 1]) {
                j++
            }
            list.add(j - i)
            i = j
        }
        val m = list.size
        val power = LongArray(m)
        power[m - 1] = list[m - 1].toLong()
        i = m - 2
        while (i >= 0) {
            power[i] = (power[i + 1] * list[i]) % MOD
            i--
        }
        if (m >= k) {
            return power[0].toInt()
        }
        val dp = Array<LongArray>(m) { LongArray(k - m + 1) }
        i = 0
        while (i < k - m + 1) {
            if (list[m - 1] + i + m > k) {
                dp[m - 1][i] = list[m - 1] - (k - m - i).toLong()
            }
            i++
        }
        i = m - 2
        while (i >= 0) {
            var sum: Long = dp[i + 1][k - m] * list[i] % MOD
            for (j in k - m downTo 0) {
                sum += dp[i + 1][j]
                if (j + list[i] > k - m) {
                    sum = (sum - dp[i + 1][k - m] + MOD) % MOD
                } else {
                    sum = (sum - dp[i + 1][j + list[i]] + MOD) % MOD
                }
                dp[i][j] = sum
            }
            i--
        }
        return dp[0][0].toInt()
    }

    companion object {
        private const val MOD = 1e9.toLong() + 7
    }
}
```