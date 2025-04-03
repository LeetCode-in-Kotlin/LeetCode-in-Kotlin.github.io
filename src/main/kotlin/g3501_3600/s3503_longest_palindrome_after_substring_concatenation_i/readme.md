[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3503\. Longest Palindrome After Substring Concatenation I

Medium

You are given two strings, `s` and `t`.

You can create a new string by selecting a **substring** from `s` (possibly empty) and a substring from `t` (possibly empty), then concatenating them **in order**.

Return the length of the **longest** palindrome that can be formed this way.

**Example 1:**

**Input:** s = "a", t = "a"

**Output:** 2

**Explanation:**

Concatenating `"a"` from `s` and `"a"` from `t` results in `"aa"`, which is a palindrome of length 2.

**Example 2:**

**Input:** s = "abc", t = "def"

**Output:** 1

**Explanation:**

Since all characters are different, the longest palindrome is any single character, so the answer is 1.

**Example 3:**

**Input:** s = "b", t = "aaaa"

**Output:** 4

**Explanation:**

Selecting "`aaaa`" from `t` is the longest palindrome, so the answer is 4.

**Example 4:**

**Input:** s = "abcde", t = "ecdba"

**Output:** 5

**Explanation:**

Concatenating `"abc"` from `s` and `"ba"` from `t` results in `"abcba"`, which is a palindrome of length 5.

**Constraints:**

*   `1 <= s.length, t.length <= 30`
*   `s` and `t` consist of lowercase English letters.

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun longestPalindrome(s: String, t: String): Int {
        var maxLen = 0
        maxLen = max(maxLen, longestPalindromicSubstring(s))
        maxLen = max(maxLen, longestPalindromicSubstring(t))
        val sLen = s.length
        val tLen = t.length
        for (i in 0..<sLen) {
            for (j in i..<sLen) {
                val m = j - i + 1
                for (k in 0..<tLen) {
                    for (l in k..<tLen) {
                        val n = l - k + 1
                        val totalLength = m + n
                        if (totalLength <= maxLen) {
                            continue
                        }
                        var isPalindrome = true
                        for (p in 0..<totalLength / 2) {
                            val q = totalLength - 1 - p
                            val c1: Char = if (p < m) {
                                s[i + p]
                            } else {
                                t[k + (p - m)]
                            }
                            val c2: Char = if (q < m) {
                                s[i + q]
                            } else {
                                t[k + (q - m)]
                            }
                            if (c1 != c2) {
                                isPalindrome = false
                                break
                            }
                        }
                        if (isPalindrome) {
                            maxLen = totalLength
                        }
                    }
                }
            }
        }
        return maxLen
    }

    private fun longestPalindromicSubstring(str: String): Int {
        var max = 0
        val len = str.length
        for (i in 0..<len) {
            for (j in i..<len) {
                var isPalin = true
                var left = i
                var right = j
                while (left < right) {
                    if (str[left] != str[right]) {
                        isPalin = false
                        break
                    }
                    left++
                    right--
                }
                if (isPalin) {
                    max = max(max, (j - i + 1))
                }
            }
        }
        return max
    }
}
```