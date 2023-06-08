[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1316\. Distinct Echo Substrings

Hard

Return the number of **distinct** non-empty substrings of `text` that can be written as the concatenation of some string with itself (i.e. it can be written as `a + a` where `a` is some string).

**Example 1:**

**Input:** text = "abcabcabc"

**Output:** 3

**Explanation:** The 3 substrings are "abcabc", "bcabca" and "cabcab".

**Example 2:**

**Input:** text = "leetcodeleetcode"

**Output:** 2

**Explanation:** The 2 substrings are "ee" and "leetcodeleetcode".

**Constraints:**

*   `1 <= text.length <= 2000`
*   `text` has only lowercase English letters.

## Solution

```kotlin
class Solution {
    fun distinctEchoSubstrings(text: String): Int {
        val n = text.length
        val dp = Array(n) { IntArray(n) }
        for (i in 0 until n) {
            var hash: Long = 0
            for (j in i until n) {
                hash = hash * PRIME + (text[j].code - 'a'.code + 1)
                hash %= MOD.toLong()
                dp[i][j] = hash.toInt()
            }
        }
        val set: MutableSet<Int> = HashSet()
        var res = 0
        for (i in 0 until n - 1) {
            var j = i
            while (2 * j - i + 1 < n) {
                if (dp[i][j] == dp[j + 1][2 * j - i + 1] && set.add(dp[i][j])) {
                    res++
                }
                j++
            }
        }
        return res
    }

    companion object {
        private const val PRIME = 101
        private const val MOD = 1000000007
    }
}
```