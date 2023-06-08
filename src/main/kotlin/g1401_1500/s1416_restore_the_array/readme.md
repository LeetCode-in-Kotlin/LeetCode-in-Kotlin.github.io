[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1416\. Restore The Array

Hard

A program was supposed to print an array of integers. The program forgot to print whitespaces and the array is printed as a string of digits `s` and all we know is that all integers in the array were in the range `[1, k]` and there are no leading zeros in the array.

Given the string `s` and the integer `k`, return _the number of the possible arrays that can be printed as_ `s` _using the mentioned program_. Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** s = "1000", k = 10000

**Output:** 1

**Explanation:** The only possible array is [1000]

**Example 2:**

**Input:** s = "1000", k = 10

**Output:** 0

**Explanation:** There cannot be an array that was printed this way and has all integer >= 1 and <= 10.

**Example 3:**

**Input:** s = "1317", k = 2000

**Output:** 8

**Explanation:** Possible arrays are [1317],[131,7],[13,17],[1,317],[13,1,7],[1,31,7],[1,3,17],[1,3,1,7]

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consists of only digits and does not contain leading zeros.
*   <code>1 <= k <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun numberOfArrays(s: String, k: Int): Int {
        // dp[i] is number of ways to print valid arrays from string s start at i
        val dp = arrayOfNulls<Int>(s.length)
        return dfs(s, k.toLong(), 0, dp)
    }

    private fun dfs(s: String, k: Long, i: Int, dp: Array<Int?>): Int {
        // base case -> Found a valid way
        if (i == s.length) return 1
        // all numbers are in range [1, k] and there are no leading zeros -> So numbers starting with 0 mean invalid!
        if (s[i] == '0') return 0
        if (dp[i] != null) return dp[i]!!
        var ans = 0
        var num: Long = 0
        for (j in i until s.length) {
            // num is the value of the substring s[i..j]
            num = num * 10 + s[j].code.toLong() - '0'.code.toLong()
            // num must be in range [1, k]
            if (num > k) break
            ans += dfs(s, k, j + 1, dp)
            ans %= 1000000007
        }
        return ans.also { dp[i] = it }
    }
}
```