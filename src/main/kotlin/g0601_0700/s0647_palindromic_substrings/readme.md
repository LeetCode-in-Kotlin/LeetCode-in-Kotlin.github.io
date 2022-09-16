[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 647\. Palindromic Substrings

Medium

Given a string `s`, return _the number of **palindromic substrings** in it_.

A string is a **palindrome** when it reads the same backward as forward.

A **substring** is a contiguous sequence of characters within the string.

**Example 1:**

**Input:** s = "abc"

**Output:** 3

**Explanation:** Three palindromic strings: "a", "b", "c".

**Example 2:**

**Input:** s = "aaa"

**Output:** 6

**Explanation:** Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".

**Constraints:**

*   `1 <= s.length <= 1000`
*   `s` consists of lowercase English letters.

## Solution

```kotlin
class Solution {
    private fun expand(a: CharArray, l: Int, r: Int, res: IntArray) {
        var left = l
        var right = r
        while (left >= 0 && right < a.size) {
            if (a[left] != a[right]) {
                return
            } else {
                res[0]++
                left--
                right++
            }
        }
    }

    fun countSubstrings(s: String): Int {
        val a = s.toCharArray()
        val res = intArrayOf(0)
        for (i in a.indices) {
            expand(a, i, i, res)
            expand(a, i, i + 1, res)
        }
        return res[0]
    }
}
```