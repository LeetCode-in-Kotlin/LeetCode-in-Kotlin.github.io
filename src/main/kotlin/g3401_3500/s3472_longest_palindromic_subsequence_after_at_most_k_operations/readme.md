[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3472\. Longest Palindromic Subsequence After at Most K Operations

Medium

You are given a string `s` and an integer `k`.

In one operation, you can replace the character at any position with the next or previous letter in the alphabet (wrapping around so that `'a'` is after `'z'`). For example, replacing `'a'` with the next letter results in `'b'`, and replacing `'a'` with the previous letter results in `'z'`. Similarly, replacing `'z'` with the next letter results in `'a'`, and replacing `'z'` with the previous letter results in `'y'`.

Return the length of the **longest palindromic subsequence** of `s` that can be obtained after performing **at most** `k` operations.

**Example 1:**

**Input:** s = "abced", k = 2

**Output:** 3

**Explanation:**

*   Replace `s[1]` with the next letter, and `s` becomes `"acced"`.
*   Replace `s[4]` with the previous letter, and `s` becomes `"accec"`.

The subsequence `"ccc"` forms a palindrome of length 3, which is the maximum.

**Example 2:**

**Input:** s = "aaazzz", k = 4

**Output:** 6

**Explanation:**

*   Replace `s[0]` with the previous letter, and `s` becomes `"zaazzz"`.
*   Replace `s[4]` with the next letter, and `s` becomes `"zaazaz"`.
*   Replace `s[3]` with the next letter, and `s` becomes `"zaaaaz"`.

The entire string forms a palindrome of length 6.

**Constraints:**

*   `1 <= s.length <= 200`
*   `1 <= k <= 200`
*   `s` consists of only lowercase English letters.

## Solution

```kotlin
import kotlin.math.abs
import kotlin.math.max
import kotlin.math.min

class Solution {
    fun longestPalindromicSubsequence(s: String, k: Int): Int {
        val n = s.length
        val arr = Array<IntArray>(26) { IntArray(26) }
        for (i in 0..25) {
            for (j in 0..25) {
                arr[i][j] = min(abs(i - j), 26 - abs(i - j))
            }
        }
        val dp = Array<Array<IntArray>>(n) { Array<IntArray>(n) { IntArray(k + 1) } }
        for (i in 0..<n) {
            for (it in 0..k) {
                dp[i][i][it] = 1
            }
        }
        for (length in 2..n) {
            for (i in 0..n - length) {
                val j = i + length - 1
                for (it in 0..k) {
                    if (s[i] == s[j]) {
                        dp[i][j][it] = 2 + dp[i + 1][j - 1][it]
                    } else {
                        val num1 = dp[i + 1][j][it]
                        val num2 = dp[i][j - 1][it]
                        val c = arr[s[i].code - 'a'.code][s[j].code - 'a'.code]
                        val num3 = if (it >= c) 2 + dp[i + 1][j - 1][it - c] else 0
                        dp[i][j][it] = max(max(num1, num2), num3)
                    }
                }
            }
        }
        return dp[0][n - 1][k]
    }
}
```