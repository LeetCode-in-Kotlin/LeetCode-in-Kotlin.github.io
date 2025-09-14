[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3677\. Count Binary Palindromic Numbers

Hard

You are given a **non-negative** integer `n`.

A **non-negative** integer is called **binary-palindromic** if its binary representation (written without leading zeros) reads the same forward and backward.

Return the number of integers `k` such that `0 <= k <= n` and the binary representation of `k` is a palindrome.

**Note:** The number 0 is considered binary-palindromic, and its representation is `"0"`.

**Example 1:**

**Input:** n = 9

**Output:** 6

**Explanation:**

The integers `k` in the range `[0, 9]` whose binary representations are palindromes are:

*   `0 → "0"`
*   `1 → "1"`
*   `3 → "11"`
*   `5 → "101"`
*   `7 → "111"`
*   `9 → "1001"`

All other values in `[0, 9]` have non-palindromic binary forms. Therefore, the count is 6.

**Example 2:**

**Input:** n = 0

**Output:** 1

**Explanation:**

Since `"0"` is a palindrome, the count is 1.

**Constraints:**

*   <code>0 <= n <= 10<sup>15</sup></code>

## Solution

```kotlin
class Solution {
    private fun makePalin(left: Long, odd: Boolean): Long {
        var left = left
        var ans = left
        if (odd) {
            left = left shr 1
        }
        while (left > 0) {
            ans = (ans shl 1) or (left and 1L)
            left = left shr 1
        }
        return ans
    }

    fun countBinaryPalindromes(n: Long): Int {
        if (n == 0L) {
            return 1
        }
        val len = 64 - java.lang.Long.numberOfLeadingZeros(n)
        var count: Long = 1
        for (i in 1..<len) {
            val half = (i + 1) / 2
            count += 1L shl (half - 1)
        }
        val half = (len + 1) / 2
        val prefix = n shr (len - half)
        val palin = makePalin(prefix, len % 2 == 1)
        count += (prefix - (1L shl (half - 1)))
        if (palin <= n) {
            ++count
        }
        return count.toInt()
    }
}
```