[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1456\. Maximum Number of Vowels in a Substring of Given Length

Medium

Given a string `s` and an integer `k`, return _the maximum number of vowel letters in any substring of_ `s` _with length_ `k`.

**Vowel letters** in English are `'a'`, `'e'`, `'i'`, `'o'`, and `'u'`.

**Example 1:**

**Input:** s = "abciiidef", k = 3

**Output:** 3

**Explanation:** The substring "iii" contains 3 vowel letters.

**Example 2:**

**Input:** s = "aeiou", k = 2

**Output:** 2

**Explanation:** Any substring of length 2 contains 2 vowels.

**Example 3:**

**Input:** s = "leetcode", k = 3

**Output:** 2

**Explanation:** "lee", "eet" and "ode" contain 2 vowels.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consists of lowercase English letters.
*   `1 <= k <= s.length`

## Solution

```kotlin
class Solution {
    private fun isVowel(c: Char): Boolean {
        return c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u'
    }

    fun maxVowels(s: String, k: Int): Int {
        var maxVowelCount = 0
        var vowelCount = 0
        var i = 0
        var j = 0
        while (j < s.length) {
            val c = s[j]
            if (isVowel(c)) {
                vowelCount++
            }
            if (j - i + 1 == k) {
                maxVowelCount = Math.max(maxVowelCount, vowelCount)
                if (isVowel(s[i])) {
                    vowelCount--
                }
                i++
            }
            j++
        }
        return maxVowelCount
    }
}
```