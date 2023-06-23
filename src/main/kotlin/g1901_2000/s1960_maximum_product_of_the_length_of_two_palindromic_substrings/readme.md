[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1960\. Maximum Product of the Length of Two Palindromic Substrings

Hard

You are given a **0-indexed** string `s` and are tasked with finding two **non-intersecting palindromic** substrings of **odd** length such that the product of their lengths is maximized.

More formally, you want to choose four integers `i`, `j`, `k`, `l` such that `0 <= i <= j < k <= l < s.length` and both the substrings `s[i...j]` and `s[k...l]` are palindromes and have odd lengths. `s[i...j]` denotes a substring from index `i` to index `j` **inclusive**.

Return _the **maximum** possible product of the lengths of the two non-intersecting palindromic substrings._

A **palindrome** is a string that is the same forward and backward. A **substring** is a contiguous sequence of characters in a string.

**Example 1:**

**Input:** s = "ababbb"

**Output:** 9

**Explanation:** Substrings "aba" and "bbb" are palindromes with odd length. product = 3 \* 3 = 9.

**Example 2:**

**Input:** s = "zaaaxbbby"

**Output:** 9

**Explanation:** Substrings "aaa" and "bbb" are palindromes with odd length. product = 3 \* 3 = 9.

**Constraints:**

*   <code>2 <= s.length <= 10<sup>5</sup></code>
*   `s` consists of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun maxProduct(s: String): Long {
        val n = s.length
        if (n == 2) {
            return 1
        }
        val len = manaCherS(s)
        val left = LongArray(n)
        var max = 1
        left[0] = max.toLong()
        for (i in 1..n - 1) {
            if (len[(i - max - 1 + i) / 2] > max) {
                max += 2
            }
            left[i] = max.toLong()
        }
        max = 1
        val right = LongArray(n)
        right[n - 1] = max.toLong()
        for (i in n - 2 downTo 0) {
            if (len[(i + max + 1 + i) / 2] > max) {
                max += 2
            }
            right[i] = max.toLong()
        }
        var res: Long = 1
        for (i in 1 until n) {
            res = Math.max(res, left[i - 1] * right[i])
        }
        return res
    }

    private fun manaCherS(s: String): IntArray {
        val len = s.length
        val p = IntArray(len)
        var c = 0
        var r = 0
        for (i in 0 until len) {
            val mirror = 2 * c - i
            if (i < r) {
                p[i] = Math.min(r - i, p[mirror])
            }
            var a = i + (1 + p[i])
            var b = i - (1 + p[i])
            while (a < len && b >= 0 && s[a] == s[b]) {
                p[i]++
                a++
                b--
            }
            if (i + p[i] > r) {
                c = i
                r = i + p[i]
            }
        }
        for (i in 0 until len) {
            p[i] = 1 + 2 * p[i]
        }
        return p
    }
}
```