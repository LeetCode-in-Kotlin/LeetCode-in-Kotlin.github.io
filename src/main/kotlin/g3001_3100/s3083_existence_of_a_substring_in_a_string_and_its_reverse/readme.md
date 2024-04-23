[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3083\. Existence of a Substring in a String and Its Reverse

Easy

Given a string `s`, find any substring of length `2` which is also present in the reverse of `s`.

Return `true` _if such a substring exists, and_ `false` _otherwise._

**Example 1:**

**Input:** s = "leetcode"

**Output:** true

**Explanation:** Substring `"ee"` is of length `2` which is also present in `reverse(s) == "edocteel"`.

**Example 2:**

**Input:** s = "abcba"

**Output:** true

**Explanation:** All of the substrings of length `2` `"ab"`, `"bc"`, `"cb"`, `"ba"` are also present in `reverse(s) == "abcba"`.

**Example 3:**

**Input:** s = "abcd"

**Output:** false

**Explanation:** There is no substring of length `2` in `s`, which is also present in the reverse of `s`.

**Constraints:**

*   `1 <= s.length <= 100`
*   `s` consists only of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun isSubstringPresent(s: String): Boolean {
        if (s.length == 1) {
            return false
        }
        val revSb = StringBuilder()
        for (i in s.length - 1 downTo 0) {
            revSb.append(s[i])
        }
        val rev = revSb.toString()
        for (i in 0 until s.length - 1) {
            val sub = s.substring(i, i + 2)
            if (rev.contains(sub)) {
                return true
            }
        }
        return false
    }
}
```