[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 214\. Shortest Palindrome

Hard

You are given a string `s`. You can convert `s` to a palindrome by adding characters in front of it.

Return _the shortest palindrome you can find by performing this transformation_.

**Example 1:**

**Input:** s = "aacecaaa"

**Output:** "aaacecaaa"

**Example 2:**

**Input:** s = "abcd"

**Output:** "dcbabcd"

**Constraints:**

*   <code>0 <= s.length <= 5 * 10<sup>4</sup></code>
*   `s` consists of lowercase English letters only.

## Solution

```kotlin
class Solution {
    fun shortestPalindrome(s: String): String {
        var i: Int
        var x: Int
        val diff: Int
        val n = s.length
        val m = (n shl 1) + 1
        val letters = CharArray(m)
        i = 0
        while (i < n) {
            letters[m - 1 - i] = s[i]
            letters[i] = letters[m - 1 - i]
            i++
        }
        letters[i] = '#'
        val lps = IntArray(m)
        lps[0] = 0
        i = 1
        while (i < m) {
            x = lps[i - 1]
            while (letters[i] != letters[x]) {
                if (x == 0) {
                    x = -1
                    break
                }
                x = lps[x - 1]
            }
            lps[i] = x + 1
            i++
        }
        diff = n - lps[m - 1]
        return if (diff == 0) {
            s
        } else {
            val builder = StringBuilder()
            i = n - 1
            while (i >= n - diff) {
                builder.append(s[i])
                i--
            }
            builder.append(s)
            builder.toString()
        }
    }
}
```