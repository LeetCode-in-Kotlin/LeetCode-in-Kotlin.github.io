[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 420\. Strong Password Checker

Hard

A password is considered strong if the below conditions are all met:

*   It has at least `6` characters and at most `20` characters.
*   It contains at least **one lowercase** letter, at least **one uppercase** letter, and at least **one digit**.
*   It does not contain three repeating characters in a row (i.e., <code>"B<ins>**aaa**</ins>bb0"</code> is weak, but <code>"B**<ins>aa</ins>**b<ins>**a**</ins>0"</code> is strong).

Given a string `password`, return _the minimum number of steps required to make `password` strong. if `password` is already strong, return `0`._

In one step, you can:

*   Insert one character to `password`,
*   Delete one character from `password`, or
*   Replace one character of `password` with another character.

**Example 1:**

**Input:** password = "a"

**Output:** 5

**Example 2:**

**Input:** password = "aA1"

**Output:** 3

**Example 3:**

**Input:** password = "1337C0d3"

**Output:** 0

**Constraints:**

*   `1 <= password.length <= 50`
*   `password` consists of letters, digits, dot `'.'` or exclamation mark `'!'`.

## Solution

```kotlin
class Solution {
    fun strongPasswordChecker(s: String): Int {
        var res = 0
        var a1 = 1
        var a2 = 1
        var d = 1
        val carr = s.toCharArray()
        val arr = IntArray(carr.size)
        var i1 = 0
        while (i1 < arr.size) {
            if (Character.isLowerCase(carr[i1])) {
                a1 = 0
            }
            if (Character.isUpperCase(carr[i1])) {
                a2 = 0
            }
            if (Character.isDigit(carr[i1])) {
                d = 0
            }
            val j = i1
            while (i1 < carr.size && carr[i1] == carr[j]) {
                i1++
            }
            arr[j] = i1 - j
        }
        val totalMissing = a1 + a2 + d
        if (arr.size < 6) {
            res += totalMissing + Math.max(0, 6 - (arr.size + totalMissing))
        } else {
            var overLen = Math.max(arr.size - 20, 0)
            var leftOver = 0
            res += overLen
            for (k in 1..2) {
                var i = 0
                while (i < arr.size && overLen > 0) {
                    if (arr[i] < 3 || arr[i] % 3 != k - 1) {
                        i++
                        continue
                    }
                    arr[i] -= Math.min(overLen, k)
                    overLen -= k
                    i++
                }
            }
            for (i in arr.indices) {
                if (arr[i] >= 3 && overLen > 0) {
                    val need = arr[i] - 2
                    arr[i] -= overLen
                    overLen -= need
                }
                if (arr[i] >= 3) {
                    leftOver += arr[i] / 3
                }
            }
            res += Math.max(totalMissing, leftOver)
        }
        return res
    }
}
```