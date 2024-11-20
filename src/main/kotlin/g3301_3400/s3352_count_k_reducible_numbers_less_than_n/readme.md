[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3352\. Count K-Reducible Numbers Less Than N

Hard

You are given a **binary** string `s` representing a number `n` in its binary form.

You are also given an integer `k`.

An integer `x` is called **k-reducible** if performing the following operation **at most** `k` times reduces it to 1:

*   Replace `x` with the **count** of set bits in its binary representation.

For example, the binary representation of 6 is `"110"`. Applying the operation once reduces it to 2 (since `"110"` has two set bits). Applying the operation again to 2 (binary `"10"`) reduces it to 1 (since `"10"` has one set bit).

Return an integer denoting the number of positive integers **less** than `n` that are **k-reducible**.

Since the answer may be too large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** s = "111", k = 1

**Output:** 3

**Explanation:**

`n = 7`. The 1-reducible integers less than 7 are 1, 2, and 4.

**Example 2:**

**Input:** s = "1000", k = 2

**Output:** 6

**Explanation:**

`n = 8`. The 2-reducible integers less than 8 are 1, 2, 3, 4, 5, and 6.

**Example 3:**

**Input:** s = "1", k = 3

**Output:** 0

**Explanation:**

There are no positive integers less than `n = 1`, so the answer is 0.

**Constraints:**

*   `1 <= s.length <= 800`
*   `s` has no leading zeros.
*   `s` consists only of the characters `'0'` and `'1'`.
*   `1 <= k <= 5`

## Solution

```kotlin
class Solution {
    fun countKReducibleNumbers(s: String, k: Int): Int {
        val n = s.length
        val reducible = IntArray(n + 1)
        for (i in 2..<reducible.size) {
            reducible[i] = 1 + reducible[Integer.bitCount(i)]
        }
        val dp = LongArray(n + 1)
        var curr = 0
        for (i in 0..<n) {
            for (j in i - 1 downTo 0) {
                dp[j + 1] += dp[j]
                dp[j + 1] %= MOD.toLong()
            }
            if (s[i] == '1') {
                dp[curr]++
                dp[curr] %= MOD.toLong()
                curr++
            }
        }
        var result: Long = 0
        for (i in 1..s.length) {
            if (reducible[i] < k) {
                result += dp[i]
                result %= MOD.toLong()
            }
        }
        return (result % MOD).toInt()
    }

    companion object {
        private val MOD = (1e9 + 7).toInt()
    }
}
```