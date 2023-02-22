[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 686\. Repeated String Match

Medium

Given two strings `a` and `b`, return _the minimum number of times you should repeat string_ `a` _so that string_ `b` _is a substring of it_. If it is impossible for `b` to be a substring of `a` after repeating it, return `-1`.

**Notice:** string `"abc"` repeated 0 times is `""`, repeated 1 time is `"abc"` and repeated 2 times is `"abcabc"`.

**Example 1:**

**Input:** a = "abcd", b = "cdabcdab"

**Output:** 3

**Explanation:** We return 3 because by repeating a three times "ab**cdabcdab**cd", b is a substring of it.

**Example 2:**

**Input:** a = "a", b = "aa"

**Output:** 2

**Constraints:**

*   <code>1 <= a.length, b.length <= 10<sup>4</sup></code>
*   `a` and `b` consist of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun repeatedStringMatch(a: String, b: String): Int {
        val existsChar = CharArray(127)
        for (chA in a.toCharArray()) {
            existsChar[chA.code] = 1.toChar()
        }
        for (chB in b.toCharArray()) {
            if (existsChar[chB.code].code < 1) {
                return -1
            }
        }
        val lenB = b.length - 1
        val sb = StringBuilder(a)
        var lenSbA = sb.length - 1
        var repeatCount = 1
        while (lenSbA < lenB) {
            sb.append(a)
            repeatCount++
            lenSbA = sb.length - 1
        }
        if (!isFound(sb, b)) {
            sb.append(a)
            repeatCount++
            return if (!isFound(sb, b)) -1 else repeatCount
        }
        return repeatCount
    }

    private fun isFound(a: StringBuilder, b: String): Boolean {
        for (i in a.indices) {
            var k = i
            var m = 0
            while (k < a.length && m < b.length) {
                if (a[k] != b[m]) {
                    break
                } else {
                    k++
                    m++
                }
            }
            if (m == b.length) {
                return true
            }
        }
        return false
    }
}
```