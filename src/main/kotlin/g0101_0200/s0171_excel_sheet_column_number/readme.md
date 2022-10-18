[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 171\. Excel Sheet Column Number

Easy

Given a string `columnTitle` that represents the column title as appears in an Excel sheet, return _its corresponding column number_.

For example:

A -> 1 B -> 2 C -> 3 ... Z -> 26 AA -> 27 AB -> 28 ...

**Example 1:**

**Input:** columnTitle = "A"

**Output:** 1

**Example 2:**

**Input:** columnTitle = "AB"

**Output:** 28

**Example 3:**

**Input:** columnTitle = "ZY"

**Output:** 701

**Constraints:**

*   `1 <= columnTitle.length <= 7`
*   `columnTitle` consists only of uppercase English letters.
*   `columnTitle` is in the range `["A", "FXSHRXW"]`.

## Solution

```kotlin
class Solution {
    fun titleToNumber(s: String): Int {
        var num = 0
        var pow = 0
        for (i in s.length - 1 downTo 0) {
            num += Math.pow(26.0, pow++.toDouble()).toInt() * (s[i].code - 'A'.code + 1)
        }
        return num
    }
}
```