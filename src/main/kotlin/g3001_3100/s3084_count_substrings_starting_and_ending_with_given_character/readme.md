[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3084\. Count Substrings Starting and Ending with Given Character

Medium

You are given a string `s` and a character `c`. Return _the total number of substrings of_ `s` _that start and end with_ `c`_._

**Example 1:**

**Input:** s = "abada", c = "a"

**Output:** 6

**Explanation:** Substrings starting and ending with `"a"` are: <code>"<ins>**a**</ins>bada"</code>, <code>"<ins>**aba**</ins>da"</code>, <code>"<ins>**abada**</ins>"</code>, <code>"ab<ins>**a**</ins>da"</code>, <code>"ab<ins>**ada**</ins>"</code>, <code>"abad<ins>**a**</ins>"</code>.

**Example 2:**

**Input:** s = "zzz", c = "z"

**Output:** 6

**Explanation:** There are a total of `6` substrings in `s` and all start and end with `"z"`.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` and `c` consist only of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun countSubstrings(s: String, c: Char): Long {
        var count: Long = 0
        for (element in s) {
            if (element == c) {
                count++
            }
        }
        return count * (count + 1) / 2
    }
}
```