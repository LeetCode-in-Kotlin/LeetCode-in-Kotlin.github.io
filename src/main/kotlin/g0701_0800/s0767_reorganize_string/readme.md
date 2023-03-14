[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 767\. Reorganize String

Medium

Given a string `s`, rearrange the characters of `s` so that any two adjacent characters are not the same.

Return _any possible rearrangement of_ `s` _or return_ `""` _if not possible_.

**Example 1:**

**Input:** s = "aab"

**Output:** "aba"

**Example 2:**

**Input:** s = "aaab"

**Output:** ""

**Constraints:**

*   `1 <= s.length <= 500`
*   `s` consists of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun reorganizeString(s: String): String {
        val hash = IntArray(26)
        for (element in s) {
            hash[element.code - 'a'.code]++
        }
        var max = 0
        var letter = 0
        for (i in hash.indices) {
            if (hash[i] > max) {
                max = hash[i]
                letter = i
            }
        }
        if (max > (s.length + 1) / 2) {
            return ""
        }
        val res = CharArray(s.length)
        var idx = 0
        while (hash[letter] > 0) {
            res[idx] = (letter + 'a'.code).toChar()
            idx += 2
            hash[letter]--
        }
        for (i in hash.indices) {
            while (hash[i] > 0) {
                if (idx >= res.size) {
                    idx = 1
                }
                res[idx] = (i + 'a'.code).toChar()
                idx += 2
                hash[i]--
            }
        }
        return String(res)
    }
}
```