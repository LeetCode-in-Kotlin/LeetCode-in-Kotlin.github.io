[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1208\. Get Equal Substrings Within Budget

Medium

You are given two strings `s` and `t` of the same length and an integer `maxCost`.

You want to change `s` to `t`. Changing the <code>i<sup>th</sup></code> character of `s` to <code>i<sup>th</sup></code> character of `t` costs `|s[i] - t[i]|` (i.e., the absolute difference between the ASCII values of the characters).

Return _the maximum length of a substring of_ `s` _that can be changed to be the same as the corresponding substring of_ `t` _with a cost less than or equal to_ `maxCost`. If there is no substring from `s` that can be changed to its corresponding substring from `t`, return `0`.

**Example 1:**

**Input:** s = "abcd", t = "bcdf", maxCost = 3

**Output:** 3

**Explanation:** "abc" of s can change to "bcd". That costs 3, so the maximum length is 3.

**Example 2:**

**Input:** s = "abcd", t = "cdef", maxCost = 3

**Output:** 1

**Explanation:** Each character in s costs 2 to change to character in t, so the maximum length is 1.

**Example 3:**

**Input:** s = "abcd", t = "acde", maxCost = 0

**Output:** 1

**Explanation:** You cannot make any change, so the maximum length is 1.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `t.length == s.length`
*   <code>0 <= maxCost <= 10<sup>6</sup></code>
*   `s` and `t` consist of only lowercase English letters.

## Solution

```kotlin
import kotlin.math.abs

class Solution {
    fun equalSubstring(s: String, t: String, maxCost: Int): Int {
        var start = 0
        var end = 0
        var currCost = 0
        var maxLength = Int.MIN_VALUE
        while (end < s.length) {
            currCost += abs(s[end].code - t[end].code)
            while (currCost > maxCost) {
                currCost -= abs(s[start].code - t[start].code)
                start++
            }
            if (end - start + 1 > maxLength) {
                maxLength = (end - start + 1).coerceAtLeast(maxLength)
            }
            end++
        }
        return maxLength
    }
}
```