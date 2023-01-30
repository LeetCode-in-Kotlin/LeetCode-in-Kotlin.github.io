[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 557\. Reverse Words in a String III

Easy

Given a string `s`, reverse the order of characters in each word within a sentence while still preserving whitespace and initial word order.

**Example 1:**

**Input:** s = "Let's take LeetCode contest"

**Output:** "s'teL ekat edoCteeL tsetnoc"

**Example 2:**

**Input:** s = "God Ding"

**Output:** "doG gniD"

**Constraints:**

*   <code>1 <= s.length <= 5 * 10<sup>4</sup></code>
*   `s` contains printable **ASCII** characters.
*   `s` does not contain any leading or trailing spaces.
*   There is **at least one** word in `s`.
*   All the words in `s` are separated by a single space.

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun reverseWords(s: String): String {
        var l: Int
        var r = 0
        val len = s.length
        val ch = s.toCharArray()
        for (i in 0..len) {
            if (i == len || ch[i] == ' ') {
                l = r
                r = i
                reverse(ch, l, r - 1)
                r++
            }
        }
        return String(ch)
    }

    private fun reverse(s: CharArray, l: Int, r: Int) {
        var l = l
        var r = r
        var c: Char
        while (r > l) {
            c = s[l]
            s[l++] = s[r]
            s[r--] = c
        }
    }
}
```