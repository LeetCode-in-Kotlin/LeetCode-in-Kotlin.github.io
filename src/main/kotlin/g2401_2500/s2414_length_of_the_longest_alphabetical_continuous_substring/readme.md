[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2414\. Length of the Longest Alphabetical Continuous Substring

Medium

An **alphabetical continuous string** is a string consisting of consecutive letters in the alphabet. In other words, it is any substring of the string `"abcdefghijklmnopqrstuvwxyz"`.

*   For example, `"abc"` is an alphabetical continuous string, while `"acb"` and `"za"` are not.

Given a string `s` consisting of lowercase letters only, return the _length of the **longest** alphabetical continuous substring._

**Example 1:**

**Input:** s = "abacaba"

**Output:** 2

**Explanation:** There are 4 distinct continuous substrings: "a", "b", "c" and "ab".

"ab" is the longest continuous substring. 

**Example 2:**

**Input:** s = "abcde"

**Output:** 5

**Explanation:** "abcde" is the longest continuous substring. 

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consists of only English lowercase letters.

## Solution

```kotlin
class Solution {
    fun longestContinuousSubstring(s: String): Int {
        var ans = 0
        var cnt = 1
        var j = 1
        while (j < s.length) {
            if (s[j].code == s[j - 1].code + 1) {
                cnt++
            } else {
                ans = ans.coerceAtLeast(cnt)
                cnt = 1
            }
            j++
        }
        return ans.coerceAtLeast(cnt)
    }
}
```