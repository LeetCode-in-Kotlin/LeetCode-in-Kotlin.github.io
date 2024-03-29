[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1446\. Consecutive Characters

Easy

The **power** of the string is the maximum length of a non-empty substring that contains only one unique character.

Given a string `s`, return _the **power** of_ `s`.

**Example 1:**

**Input:** s = "leetcode"

**Output:** 2

**Explanation:** The substring "ee" is of length 2 with the character 'e' only.

**Example 2:**

**Input:** s = "abbcccddddeeeeedcba"

**Output:** 5

**Explanation:** The substring "eeeee" is of length 5 with the character 'e' only.

**Constraints:**

*   `1 <= s.length <= 500`
*   `s` consists of only lowercase English letters.

## Solution

```kotlin
class Solution {
    fun maxPower(s: String): Int {
        var max = 0
        var i = 0
        while (i < s.length) {
            val start = i
            while (i + 1 < s.length && s[i] == s[i + 1]) {
                i++
            }
            max = Math.max(max, i - start + 1)
            i++
        }
        return max
    }
}
```