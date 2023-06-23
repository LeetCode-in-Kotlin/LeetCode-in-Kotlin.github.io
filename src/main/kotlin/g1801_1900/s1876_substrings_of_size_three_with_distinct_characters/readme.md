[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1876\. Substrings of Size Three with Distinct Characters

Easy

A string is **good** if there are no repeated characters.

Given a string `s`, return _the number of **good substrings** of length **three** in_ `s`.

Note that if there are multiple occurrences of the same substring, every occurrence should be counted.

A **substring** is a contiguous sequence of characters in a string.

**Example 1:**

**Input:** s = "xyzzaz"

**Output:** 1

**Explanation:** There are 4 substrings of size 3: "xyz", "yzz", "zza", and "zaz". The only good substring of length 3 is "xyz".

**Example 2:**

**Input:** s = "aababcabc"

**Output:** 4

**Explanation:** There are 7 substrings of size 3: "aab", "aba", "bab", "abc", "bca", "cab", and "abc". The good substrings are "abc", "bca", "cab", and "abc".

**Constraints:**

*   `1 <= s.length <= 100`
*   `s` consists of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun countGoodSubstrings(s: String): Int {
        var count = 0
        for (i in 0 until s.length - 2) {
            val candidate = s.substring(i, i + 3)
            if (candidate[0] != candidate[1] && candidate[0] != candidate[2] && candidate[1] != candidate[2]) {
                count++
            }
        }
        return count
    }
}
```