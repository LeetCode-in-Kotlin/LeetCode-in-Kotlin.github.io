[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3900\. Longest Balanced Substring After One Swap

Medium

You are given a binary string `s` consisting only of characters `'0'` and `'1'`.

A string is **balanced** if it contains an **equal** number of `'0'`s and `'1'`s.

You can perform **at most one** swap between any two characters in `s`. Then, you select a **balanced** substring from `s`.

Return an integer representing the **maximum** length of the **balanced** substring you can select.

**Example 1:**

**Input:** s = "100001"

**Output:** 4

**Explanation:**

*   Swap <code>"10<ins>**0**</ins>00<ins>**1**</ins>"</code>. The string becomes `"101000"`.
*   Select the substring <code>"<ins>**1010**</ins>00"</code>, which is balanced because it has two `'0'`s and two `'1'`s.

**Example 2:**

**Input:** s = "111"

**Output:** 0

**Explanation:**

*   Choose not to perform any swaps.
*   Select the empty substring, which is balanced because it has zero `'0'`s and zero `'1'`s.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consists only of the characters `'0'` and `'1'`.

## Solution

```kotlin
import kotlin.math.min

class Solution {
    fun longestBalanced(s: String): Int {
        val arr = s.toCharArray()
        val n = arr.size
        var bal = n + 1
        var ans = 0
        val nextIndex = IntArray(n)
        val balIndex = IntArray(2 * n + 3)
        balIndex.fill(n + 1)
        for (i in n - 1 downTo 0) {
            bal += (('0'.code xor arr[i].code) shl 1) - 1
            nextIndex[i] = balIndex[bal]
            balIndex[bal] = i
        }
        if (bal == n + 1) {
            return n
        }
        val zeros = (2 * n + 1 - bal) / 2
        val maxLength = 2 * min(zeros, n - zeros)
        var i = 1
        while (i <= n && ans < maxLength) {
            bal += (('1'.code xor arr[i - 1].code) shl 1) - 1
            if (i - balIndex[bal] > ans) {
                ans = i - balIndex[bal]
            }
            if (balIndex[bal - 2] < i - maxLength) {
                balIndex[bal - 2] = nextIndex[balIndex[bal - 2]]
            }
            if (i - balIndex[bal - 2] > ans) {
                ans = i - balIndex[bal - 2]
            }
            if (balIndex[bal + 2] < i - maxLength) {
                balIndex[bal + 2] = nextIndex[balIndex[bal + 2]]
            }
            if (i - balIndex[bal + 2] > ans) {
                ans = i - balIndex[bal + 2]
            }
            i++
        }
        return ans
    }
}
```