[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2186\. Minimum Number of Steps to Make Two Strings Anagram II

Medium

You are given two strings `s` and `t`. In one step, you can append **any character** to either `s` or `t`.

Return _the minimum number of steps to make_ `s` _and_ `t` _**anagrams** of each other._

An **anagram** of a string is a string that contains the same characters with a different (or the same) ordering.

**Example 1:**

**Input:** s = "**lee**tco**de**", t = "co**a**t**s**"

**Output:** 7

**Explanation:**

- In 2 steps, we can append the letters in "as" onto s = "leetcode", forming s = "leetcode**as**".

- In 5 steps, we can append the letters in "leede" onto t = "coats", forming t = "coats**leede**".

"leetcodeas" and "coatsleede" are now anagrams of each other.

We used a total of 2 + 5 = 7 steps.

It can be shown that there is no way to make them anagrams of each other with less than 7 steps. 

**Example 2:**

**Input:** s = "night", t = "thing"

**Output:** 0

**Explanation:** The given strings are already anagrams of each other. Thus, we do not need any further steps. 

**Constraints:**

*   <code>1 <= s.length, t.length <= 2 * 10<sup>5</sup></code>
*   `s` and `t` consist of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun minSteps(s: String, t: String): Int {
        val a = IntArray(26)
        for (i in 0 until s.length) {
            a[s[i].code - 'a'.code]++
        }
        for (i in 0 until t.length) {
            a[t[i].code - 'a'.code]--
        }
        var sum = 0
        for (j in a) {
            sum += Math.abs(j)
        }
        return sum
    }
}
```