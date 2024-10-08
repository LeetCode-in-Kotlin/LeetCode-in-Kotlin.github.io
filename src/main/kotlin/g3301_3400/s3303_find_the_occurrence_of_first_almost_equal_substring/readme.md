[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3303\. Find the Occurrence of First Almost Equal Substring

Hard

You are given two strings `s` and `pattern`.

A string `x` is called **almost equal** to `y` if you can change **at most** one character in `x` to make it _identical_ to `y`.

Return the **smallest** _starting index_ of a substring in `s` that is **almost equal** to `pattern`. If no such index exists, return `-1`.

A **substring** is a contiguous **non-empty** sequence of characters within a string.

**Example 1:**

**Input:** s = "abcdefg", pattern = "bcdffg"

**Output:** 1

**Explanation:**

The substring `s[1..6] == "bcdefg"` can be converted to `"bcdffg"` by changing `s[4]` to `"f"`.

**Example 2:**

**Input:** s = "ababbababa", pattern = "bacaba"

**Output:** 4

**Explanation:**

The substring `s[4..9] == "bababa"` can be converted to `"bacaba"` by changing `s[6]` to `"c"`.

**Example 3:**

**Input:** s = "abcd", pattern = "dba"

**Output:** \-1

**Example 4:**

**Input:** s = "dde", pattern = "d"

**Output:** 0

**Constraints:**

*   <code>1 <= pattern.length < s.length <= 3 * 10<sup>5</sup></code>
*   `s` and `pattern` consist only of lowercase English letters.

**Follow-up:** Could you solve the problem if **at most** `k` **consecutive** characters can be changed?

## Solution

```kotlin
import kotlin.math.abs

class Solution {
    fun minStartingIndex(s: String, pattern: String): Int {
        val n = s.length
        var left = 0
        var right = 0
        val f1 = IntArray(26)
        val f2 = IntArray(26)
        for (ch in pattern.toCharArray()) {
            f2[ch.code - 'a'.code]++
        }
        while (right < n) {
            val ch = s[right]
            f1[ch.code - 'a'.code]++
            if (right - left + 1 == pattern.length + 1) {
                f1[s[left].code - 'a'.code]--
                left += 1
            }
            if (right - left + 1 == pattern.length && check(f1, f2, left, s, pattern)) {
                return left
            }
            right += 1
        }
        return -1
    }

    private fun check(f1: IntArray, f2: IntArray, left: Int, s: String, pattern: String): Boolean {
        var cnt = 0
        for (i in 0..25) {
            if (f1[i] != f2[i]) {
                if ((abs((f1[i] - f2[i])) > 1) || (abs(f1[i] - f2[i]) != 1 && cnt == 2)) {
                    return false
                }
                cnt += 1
            }
        }
        cnt = 0
        var start = 0
        var end = pattern.length - 1
        while (start <= end) {
            if (s[start + left] != pattern[start]) {
                cnt += 1
            }
            if (start + left != left + end && s[left + end] != pattern[end]) {
                cnt += 1
            }
            if (cnt >= 2) {
                return false
            }
            start++
            end--
        }
        return true
    }
}
```