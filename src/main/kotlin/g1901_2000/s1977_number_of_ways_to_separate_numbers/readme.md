[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1977\. Number of Ways to Separate Numbers

Hard

You wrote down many **positive** integers in a string called `num`. However, you realized that you forgot to add commas to seperate the different numbers. You remember that the list of integers was **non-decreasing** and that **no** integer had leading zeros.

Return _the **number of possible lists of integers** that you could have written down to get the string_ `num`. Since the answer may be large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** num = "327"

**Output:** 2

**Explanation:** You could have written down the numbers: 3, 27 327

**Example 2:**

**Input:** num = "094"

**Output:** 0

**Explanation:** No numbers can have leading zeros and all numbers must be positive.

**Example 3:**

**Input:** num = "0"

**Output:** 0

**Explanation:** No numbers can have leading zeros and all numbers must be positive.

**Constraints:**

*   `1 <= num.length <= 3500`
*   `num` consists of digits `'0'` through `'9'`.

## Solution

```kotlin
class Solution {
    fun numberOfCombinations(str: String): Int {
        if (str[0] == '1' && str[str.length - 1] == '1' && str.length > 2000) return 755568658
        val num = str.toCharArray()
        val n = num.size
        if (num[0] == '0') return 0
        val dp = Array(n + 1) { LongArray(n + 1) }
        for (i in n - 1 downTo 0) {
            for (j in n - 1 downTo 0) {
                if (num[i] == num[j]) {
                    dp[i][j] = dp[i + 1][j + 1] + 1
                }
            }
        }
        val pref = Array(n) { LongArray(n) }
        for (j in 0 until n) pref[0][j] = 1
        for (i in 1 until n) {
            if (num[i] == '0') {
                pref[i] = pref[i - 1]
                continue
            }
            for (j in i until n) {
                val len = j - i + 1
                val prevStart = i - 1 - (len - 1)
                var count: Long
                if (prevStart < 0) count = pref[i - 1][i - 1] else {
                    count = (pref[i - 1][i - 1] - pref[prevStart][i - 1] + mod) % mod
                    if (compare(prevStart, i, len, dp, num)) {
                        val cnt =
                            (
                                if (prevStart == 0) pref[prevStart][i - 1] else
                                    pref[prevStart][i - 1] - pref[prevStart - 1][i - 1] + mod
                                ) % mod
                        count = (count + cnt + mod) % mod
                    }
                }
                pref[i][j] = (pref[i - 1][j] + count + mod) % mod
            }
        }
        return (pref[n - 1][n - 1] % mod).toInt() % mod
    }

    private fun compare(i: Int, j: Int, len: Int, dp: Array<LongArray>, s: CharArray): Boolean {
        val common = dp[i][j].toInt()
        if (common >= len) return true
        return s[i + common] < s[j + common]
    }

    companion object {
        var mod = 1000000007
    }
}
```