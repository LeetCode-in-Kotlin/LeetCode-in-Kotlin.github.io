[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2484\. Count Palindromic Subsequences

Hard

Given a string of digits `s`, return _the number of **palindromic subsequences** of_ `s` _having length_ `5`. Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Note:**

*   A string is **palindromic** if it reads the same forward and backward.
*   A **subsequence** is a string that can be derived from another string by deleting some or no characters without changing the order of the remaining characters.

**Example 1:**

**Input:** s = "103301"

**Output:** 2

**Explanation:** There are 6 possible subsequences of length 5: "10330","10331","10301","10301","13301","03301". Two of them (both equal to "10301") are palindromic.

**Example 2:**

**Input:** s = "0000000"

**Output:** 21

**Explanation:** All 21 subsequences are "00000", which is palindromic.

**Example 3:**

**Input:** s = "9999900000"

**Output:** 2

**Explanation:** The only two palindromic subsequences are "99999" and "00000".

**Constraints:**

*   <code>1 <= s.length <= 10<sup>4</sup></code>
*   `s` consists of digits.

## Solution

```kotlin
class Solution {
    fun countPalindromes(s: String): Int {
        val n = s.length
        var ans: Long = 0
        val mod = 1e9.toInt() + 7
        val count = IntArray(10)
        for (i in 0 until n) {
            var m: Long = 0
            for (j in n - 1 downTo i + 1) {
                if (s[i] == s[j]) {
                    ans += m * (j - i - 1)
                    ans = ans % mod
                }
                m += count[s[j].code - '0'.code].toLong()
            }
            count[s[i].code - '0'.code]++
        }
        return ans.toInt()
    }
}
```