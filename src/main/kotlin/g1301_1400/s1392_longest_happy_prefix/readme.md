[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1392\. Longest Happy Prefix

Hard

A string is called a **happy prefix** if is a **non-empty** prefix which is also a suffix (excluding itself).

Given a string `s`, return _the **longest happy prefix** of_ `s`. Return an empty string `""` if no such prefix exists.

**Example 1:**

**Input:** s = "level"

**Output:** "l"

**Explanation:** s contains 4 prefix excluding itself ("l", "le", "lev", "leve"), and suffix ("l", "el", "vel", "evel"). The largest prefix which is also suffix is given by "l".

**Example 2:**

**Input:** s = "ababab"

**Output:** "abab"

**Explanation:** "abab" is the largest prefix which is also suffix. They can overlap in the original string.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` contains only lowercase English letters.

## Solution

```kotlin
class Solution {
    fun longestPrefix(s: String): String {
        val times = 2
        var prefixHash: Long = 0
        var suffixHash: Long = 0
        var multiplier: Long = 1
        var len: Long = 0
        // use some large prime as a modulo to avoid overflow errors, e.g. 10 ^ 9 + 7.
        val mod: Long = 1000000007
        for (i in 0 until s.length - 1) {
            prefixHash = (prefixHash * times + s[i].code.toLong()) % mod
            suffixHash = (multiplier * s[s.length - i - 1].code.toLong() + suffixHash) % mod
            if (prefixHash == suffixHash) {
                len = i.toLong() + 1
            }
            multiplier = multiplier * times % mod
        }
        return s.substring(0, len.toInt())
    }
}
```