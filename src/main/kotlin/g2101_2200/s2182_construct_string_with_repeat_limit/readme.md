[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2182\. Construct String With Repeat Limit

Medium

You are given a string `s` and an integer `repeatLimit`. Construct a new string `repeatLimitedString` using the characters of `s` such that no letter appears **more than** `repeatLimit` times **in a row**. You do **not** have to use all characters from `s`.

Return _the **lexicographically largest**_ `repeatLimitedString` _possible_.

A string `a` is **lexicographically larger** than a string `b` if in the first position where `a` and `b` differ, string `a` has a letter that appears later in the alphabet than the corresponding letter in `b`. If the first `min(a.length, b.length)` characters do not differ, then the longer string is the lexicographically larger one.

**Example 1:**

**Input:** s = "cczazcc", repeatLimit = 3

**Output:** "zzcccac"

**Explanation:** We use all of the characters from s to construct the repeatLimitedString "zzcccac".

The letter 'a' appears at most 1 time in a row.

The letter 'c' appears at most 3 times in a row.

The letter 'z' appears at most 2 times in a row.

Hence, no letter appears more than repeatLimit times in a row and the string is a valid repeatLimitedString.

The string is the lexicographically largest repeatLimitedString possible so we return "zzcccac".

Note that the string "zzcccca" is lexicographically larger but the letter 'c' appears more than 3 times in a row, so it is not a valid repeatLimitedString. 

**Example 2:**

**Input:** s = "aababab", repeatLimit = 2

**Output:** "bbabaa"

**Explanation:** We use only some of the characters from s to construct the repeatLimitedString "bbabaa".

The letter 'a' appears at most 2 times in a row. The letter 'b' appears at most 2 times in a row.

Hence, no letter appears more than repeatLimit times in a row and the string is a valid repeatLimitedString.

The string is the lexicographically largest repeatLimitedString possible so we return "bbabaa".

Note that the string "bbabaaa" is lexicographically larger but the letter 'a' appears more than 2 times in a row, so it is not a valid repeatLimitedString. 

**Constraints:**

*   <code>1 <= repeatLimit <= s.length <= 10<sup>5</sup></code>
*   `s` consists of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun repeatLimitedString(s: String, repeatLimit: Int): String {
        val result = CharArray(s.length)
        val freq = IntArray(128)
        var index = 0
        for (c in s.toCharArray()) {
            freq[c.code]++
        }
        var max = 'z'
        var second = 'y'
        while (true) {
            while (max >= 'a' && freq[max.code] == 0) {
                max--
            }
            if (max < 'a') {
                break
            }
            second = Math.min(max.code - 1, second.code).toChar()
            var count = Math.min(freq[max.code], repeatLimit)
            freq[max.code] -= count
            while (count-- > 0) {
                result[index++] = max
            }
            if (freq[max.code] == 0) {
                max = second--
                continue
            }
            while (second >= 'a' && freq[second.code] == 0) {
                second--
            }
            if (second < 'a') {
                break
            }
            result[index++] = second
            freq[second.code]--
        }
        return String(result, 0, index)
    }
}
```