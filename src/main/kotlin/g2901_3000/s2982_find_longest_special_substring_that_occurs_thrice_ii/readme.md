[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2982\. Find Longest Special Substring That Occurs Thrice II

Medium

You are given a string `s` that consists of lowercase English letters.

A string is called **special** if it is made up of only a single character. For example, the string `"abc"` is not special, whereas the strings `"ddd"`, `"zz"`, and `"f"` are special.

Return _the length of the **longest special substring** of_ `s` _which occurs **at least thrice**_, _or_ `-1` _if no special substring occurs at least thrice_.

A **substring** is a contiguous **non-empty** sequence of characters within a string.

**Example 1:**

**Input:** s = "aaaa"

**Output:** 2

**Explanation:** The longest special substring which occurs thrice is "aa": substrings "<ins>**aa**</ins>aa", "a<ins>**aa**</ins>a", and "aa<ins>**aa**</ins>".

It can be shown that the maximum length achievable is 2. 

**Example 2:**

**Input:** s = "abcdef"

**Output:** -1

**Explanation:** There exists no special substring which occurs at least thrice. Hence return -1. 

**Example 3:**

**Input:** s = "abcaba"

**Output:** 1

**Explanation:** The longest special substring which occurs thrice is "a": substrings "<ins>**a**</ins>bcaba", "abc<ins>**a**</ins>ba", and "abcab<ins>**a**</ins>".

It can be shown that the maximum length achievable is 1. 

**Constraints:**

*   <code>3 <= s.length <= 5 * 10<sup>5</sup></code>
*   `s` consists of only lowercase English letters.

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun maximumLength(s: String): Int {
        val arr = Array(26) { IntArray(4) }
        var prev = s[0]
        var count = 1
        var max = 0
        for (index in 1 until s.length) {
            if (s[index] != prev) {
                val ints = arr[prev.code - 'a'.code]
                updateArr(count, ints)
                prev = s[index]
                count = 1
            } else {
                count++
            }
        }
        updateArr(count, arr[prev.code - 'a'.code])
        for (values in arr) {
            if (values[0] != 0) {
                max = if (values[1] >= 3) {
                    max(max, values[0])
                } else if (values[1] == 2 || values[2] == values[0] - 1) {
                    max(max, (values[0] - 1))
                } else {
                    max(max, (values[0] - 2))
                }
            }
        }
        return if (max == 0) -1 else max
    }

    private fun updateArr(count: Int, ints: IntArray) {
        if (ints[0] == count) {
            ints[1]++
        } else if (ints[0] < count) {
            ints[3] = ints[1]
            ints[2] = ints[0]
            ints[0] = count
            ints[1] = 1
        } else if (ints[2] < count) {
            ints[2] = count
            ints[3] = 1
        }
    }
}
```