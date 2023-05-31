[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1163\. Last Substring in Lexicographical Order

Hard

Given a string `s`, return _the last substring of_ `s` _in lexicographical order_.

**Example 1:**

**Input:** s = "abab"

**Output:** "bab"

**Explanation:** The substrings are ["a", "ab", "aba", "abab", "b", "ba", "bab"]. The lexicographically maximum substring is "bab".

**Example 2:**

**Input:** s = "leetcode"

**Output:** "tcode"

**Constraints:**

*   <code>1 <= s.length <= 4 * 10<sup>5</sup></code>
*   `s` contains only lowercase English letters.

## Solution

```kotlin
class Solution {
    fun lastSubstring(s: String): String {
        var i = 0
        var j = 1
        var k = 0
        val n = s.length
        val ca = s.toCharArray()
        while (j + k < n) {
            if (ca[i + k] == ca[j + k]) {
                k++
            } else if (ca[i + k] > ca[j + k]) {
                j += k + 1
                k = 0
            } else {
                i = Math.max(i + k + 1, j)
                j = i + 1
                k = 0
            }
        }
        return s.substring(i)
    }
}
```