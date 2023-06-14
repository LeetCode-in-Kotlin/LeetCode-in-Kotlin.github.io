[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1525\. Number of Good Ways to Split a String

Medium

You are given a string `s`.

A split is called **good** if you can split `s` into two non-empty strings <code>s<sub>left</sub></code> and <code>s<sub>right</sub></code> where their concatenation is equal to `s` (i.e., <code>s<sub>left</sub> + s<sub>right</sub> = s</code>) and the number of distinct letters in <code>s<sub>left</sub></code> and <code>s<sub>right</sub></code> is the same.

Return _the number of **good splits** you can make in `s`_.

**Example 1:**

**Input:** s = "aacaba"

**Output:** 2

**Explanation:** There are 5 ways to split `"aacaba"` and 2 of them are good. 

("a", "acaba") Left string and right string contains 1 and 3 different letters respectively. 

("aa", "caba") Left string and right string contains 1 and 3 different letters respectively. 

("aac", "aba") Left string and right string contains 2 and 2 different letters respectively (good split). 

("aaca", "ba") Left string and right string contains 2 and 2 different letters respectively (good split). 

("aacab", "a") Left string and right string contains 3 and 1 different letters respectively.

**Example 2:**

**Input:** s = "abcd"

**Output:** 1

**Explanation:** Split the string as follows ("ab", "cd").

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consists of only lowercase English letters.

## Solution

```kotlin
class Solution {
    fun numSplits(s: String): Int {
        val hs = HashSet<Char>()
        val dp1 = IntArray(s.length - 1)
        val dp2 = IntArray(s.length - 1)
        for (i in 0 until s.length - 1) {
            val ch = s[i]
            hs.add(ch)
            dp1[i] = hs.size
        }
        val hm = HashSet<Char>()
        for (i in s.length - 1 downTo 1) {
            val ch = s[i]
            hm.add(ch)
            dp2[i - 1] = hm.size
        }
        var count = 0
        for (i in 0 until s.length - 1) {
            if (dp1[i] == dp2[i]) {
                count++
            }
        }
        return count
    }
}
```