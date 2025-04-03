[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3504\. Longest Palindrome After Substring Concatenation II

Hard

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

*   `1 <= s.length, t.length <= 1000`
*   `s` and `t` consist of lowercase English letters.

## Solution

```kotlin
import kotlin.math.max

class Solution {
    private lateinit var sPa: IntArray
    private lateinit var tPa: IntArray
    private lateinit var ss: CharArray
    private lateinit var tt: CharArray

    fun longestPalindrome(s: String, t: String): Int {
        val sLen = s.length
        val tLen = t.length
        ss = s.toCharArray()
        tt = t.toCharArray()
        val palindrome = Array<IntArray?>(sLen) { IntArray(tLen + 1) }
        sPa = IntArray(sLen)
        tPa = IntArray(tLen)
        var maxLen = 1
        for (j in 0..<tLen) {
            if (ss[0] == tt[j]) {
                palindrome[0]!![j] = 2
                sPa[0] = 2
                tPa[j] = 2
                maxLen = 2
            }
        }
        for (i in 1..<sLen) {
            for (j in 0..<tLen) {
                if (ss[i] == tt[j]) {
                    palindrome[i]!![j] = 2 + palindrome[i - 1]!![j + 1]
                    sPa[i] = max(sPa[i], palindrome[i]!![j])
                    tPa[j] = max(tPa[j], palindrome[i]!![j])
                    maxLen = max(maxLen, palindrome[i]!![j])
                }
            }
        }
        for (i in 0..<sLen - 1) {
            val len = maxS(i, i + 1)
            maxLen = max(maxLen, len)
        }
        for (i in 1..<sLen) {
            val len = maxS(i - 1, i + 1) + 1
            maxLen = max(maxLen, len)
        }
        for (j in 0..<tLen - 1) {
            val len = maxT(j, j + 1)
            maxLen = max(maxLen, len)
        }
        for (j in 0..<tLen - 1) {
            val len = maxT(j - 1, j + 1) + 1
            maxLen = max(maxLen, len)
        }
        return maxLen
    }

    private fun maxS(left: Int, right: Int): Int {
        var left = left
        var right = right
        var len = 0
        while (left >= 0 && right < ss.size && ss[left] == ss[right]) {
            len += 2
            left--
            right++
        }
        if (left >= 0) {
            len += sPa[left]
        }
        return len
    }

    private fun maxT(left: Int, right: Int): Int {
        var left = left
        var right = right
        var len = 0
        while (left >= 0 && right < tt.size && tt[left] == tt[right]) {
            len += 2
            left--
            right++
        }
        if (right < tt.size) {
            len += tPa[right]
        }
        return len
    }
}
```