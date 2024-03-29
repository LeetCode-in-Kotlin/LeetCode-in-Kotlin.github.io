[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 828\. Count Unique Characters of All Substrings of a Given String

Hard

Let's define a function `countUniqueChars(s)` that returns the number of unique characters on `s`.

*   For example, calling `countUniqueChars(s)` if `s = "LEETCODE"` then `"L"`, `"T"`, `"C"`, `"O"`, `"D"` are the unique characters since they appear only once in `s`, therefore `countUniqueChars(s) = 5`.

Given a string `s`, return the sum of `countUniqueChars(t)` where `t` is a substring of `s`. The test cases are generated such that the answer fits in a 32-bit integer.

Notice that some substrings can be repeated so in this case you have to count the repeated ones too.

**Example 1:**

**Input:** s = "ABC"

**Output:** 10

**Explanation:** All possible substrings are: "A","B","C","AB","BC" and "ABC".

Every substring is composed with only unique letters. 

Sum of lengths of all substring is 1 + 1 + 1 + 2 + 2 + 3 = 10

**Example 2:**

**Input:** s = "ABA"

**Output:** 8

**Explanation:** The same as example 1, except `countUniqueChars`("ABA") = 1.

**Example 3:**

**Input:** s = "LEETCODE"

**Output:** 92

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consists of uppercase English letters only.

## Solution

```kotlin
class Solution {
    fun uniqueLetterString(s: String): Int {
        // Store last index of a character.
        val lastCharIdx = IntArray(26)
        // Store last to last index of a character.
        // Eg. For ABA - lastCharIdx = 2, prevLastCharIdx = 0.
        val prevLastCharIdx = IntArray(26)
        lastCharIdx.fill(-1)
        prevLastCharIdx.fill(-1)
        val len = s.length
        val dp = IntArray(len)
        var account = 1
        dp[0] = 1
        lastCharIdx[s[0].code - 'A'.code] = 0
        prevLastCharIdx[s[0].code - 'A'.code] = 0
        for (i in 1 until len) {
            val ch = s[i]
            val chIdx = ch.code - 'A'.code
            val lastSeenIdx = lastCharIdx[chIdx]
            val prevLastIdx = prevLastCharIdx[chIdx]
            dp[i] = dp[i - 1] + 1
            if (lastSeenIdx == -1) {
                dp[i] += i
            } else {
                // Add count for curr index from index of last char appearance.
                dp[i] += i - lastSeenIdx - 1
                if (prevLastIdx == lastSeenIdx) {
                    // if last char idx is the only appearance of the char from left so far,
                    // substrings that overlap [0, lastSeenIdx] will need count to be discounted, as
                    // current char will cause duplication.
                    dp[i] -= lastSeenIdx + 1
                } else {
                    dp[i] -= lastSeenIdx - prevLastIdx
                }
            }
            prevLastCharIdx[chIdx] = lastSeenIdx
            lastCharIdx[chIdx] = i
            account += dp[i]
        }
        return account
    }
}
```