[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3292\. Minimum Number of Valid Strings to Form Target II

Hard

You are given an array of strings `words` and a string `target`.

A string `x` is called **valid** if `x` is a prefix of **any** string in `words`.

Return the **minimum** number of **valid** strings that can be _concatenated_ to form `target`. If it is **not** possible to form `target`, return `-1`.

A prefix of a string is a substring that starts from the beginning of the string and extends to any point within it.

**Example 1:**

**Input:** words = ["abc","aaaaa","bcdef"], target = "aabcdabc"

**Output:** 3

**Explanation:**

The target string can be formed by concatenating:

*   Prefix of length 2 of `words[1]`, i.e. `"aa"`.
*   Prefix of length 3 of `words[2]`, i.e. `"bcd"`.
*   Prefix of length 3 of `words[0]`, i.e. `"abc"`.

**Example 2:**

**Input:** words = ["abababab","ab"], target = "ababaababa"

**Output:** 2

**Explanation:**

The target string can be formed by concatenating:

*   Prefix of length 5 of `words[0]`, i.e. `"ababa"`.
*   Prefix of length 5 of `words[0]`, i.e. `"ababa"`.

**Example 3:**

**Input:** words = ["abcdef"], target = "xyz"

**Output:** \-1

**Constraints:**

*   `1 <= words.length <= 100`
*   <code>1 <= words[i].length <= 5 * 10<sup>4</sup></code>
*   The input is generated such that <code>sum(words[i].length) <= 10<sup>5</sup></code>.
*   `words[i]` consists only of lowercase English letters.
*   <code>1 <= target.length <= 5 * 10<sup>4</sup></code>
*   `target` consists only of lowercase English letters.

## Solution

```kotlin
import java.util.ArrayList
import kotlin.math.min

class Solution {
    fun minValidStrings(words: Array<String>, target: String): Int {
        val n = target.length
        val dp = IntArray(n + 1)
        dp.fill(Int.Companion.MAX_VALUE)
        dp[0] = 0
        val matches: MutableList<MutableList<Int>> = ArrayList<MutableList<Int>>(n)
        for (i in 0 until n) {
            matches.add(ArrayList<Int>())
        }
        val targetChars = target.toCharArray()
        for (word in words) {
            val wordChars = word.toCharArray()
            val m = wordChars.size
            val pi = IntArray(m)
            var i1 = 1
            var j1 = 0
            while (i1 < m) {
                while (j1 > 0 && wordChars[i1] != wordChars[j1]) {
                    j1 = pi[j1 - 1]
                }
                if (wordChars[i1] == wordChars[j1]) {
                    j1++
                }
                pi[i1] = j1
                i1++
            }
            var i = 0
            var j = 0
            while (i < n) {
                while (j > 0 && targetChars[i] != wordChars[j]) {
                    j = pi[j - 1]
                }
                if (targetChars[i] == wordChars[j]) {
                    j++
                }
                if (j > 0) {
                    matches[i - j + 1].add(j)
                    if (j == m) {
                        j = pi[j - 1]
                    }
                }
                i++
            }
        }
        for (i in 0 until n) {
            if (dp[i] == Int.Companion.MAX_VALUE) {
                continue
            }
            for (len in matches[i]) {
                if (i + len <= n) {
                    dp[i + len] = min(dp[i + len], (dp[i] + 1))
                }
            }
        }
        return if (dp[n] == Int.Companion.MAX_VALUE) -1 else dp[n]
    }
}
```