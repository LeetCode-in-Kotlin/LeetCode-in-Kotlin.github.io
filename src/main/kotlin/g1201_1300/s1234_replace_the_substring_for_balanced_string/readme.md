[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1234\. Replace the Substring for Balanced String

Medium

You are given a string s of length `n` containing only four kinds of characters: `'Q'`, `'W'`, `'E'`, and `'R'`.

A string is said to be **balanced** if each of its characters appears `n / 4` times where `n` is the length of the string.

Return _the minimum length of the substring that can be replaced with **any** other string of the same length to make_ `s` _**balanced**_. If s is already **balanced**, return `0`.

**Example 1:**

**Input:** s = "QWER"

**Output:** 0

**Explanation:** s is already balanced.

**Example 2:**

**Input:** s = "QQWE"

**Output:** 1

**Explanation:** We need to replace a 'Q' to 'R', so that "RQWE" (or "QRWE") is balanced.

**Example 3:**

**Input:** s = "QQQW"

**Output:** 2

**Explanation:** We can replace the first "QQ" to "ER".

**Constraints:**

*   `n == s.length`
*   <code>4 <= n <= 10<sup>5</sup></code>
*   `n` is a multiple of `4`.
*   `s` contains only `'Q'`, `'W'`, `'E'`, and `'R'`.

## Solution

```kotlin
class Solution {
    fun balancedString(s: String): Int {
        val n = s.length
        var ans = n
        var excess = 0
        val cnt = IntArray(128)
        cnt['R'.code] = -n / 4
        cnt['E'.code] = cnt['R'.code]
        cnt['W'.code] = cnt['E'.code]
        cnt['Q'.code] = cnt['W'.code]
        for (ch in s.toCharArray()) {
            if (++cnt[ch.code] == 1) {
                excess++
            }
        }
        if (excess == 0) {
            return 0
        }
        var i = 0
        var j = 0
        while (i < n) {
            if (--cnt[s[i].code] == 0) {
                excess--
            }
            while (excess == 0) {
                if (++cnt[s[j].code] == 1) {
                    excess++
                }
                ans = Math.min(i - j + 1, ans)
                j++
            }
            i++
        }
        return ans
    }
}
```