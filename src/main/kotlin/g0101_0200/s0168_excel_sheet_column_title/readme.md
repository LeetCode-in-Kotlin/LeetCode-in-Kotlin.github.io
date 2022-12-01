[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 168\. Excel Sheet Column Title

Easy

Given an integer `columnNumber`, return _its corresponding column title as it appears in an Excel sheet_.

For example:

A -> 1 B -> 2 C -> 3 ... Z -> 26 AA -> 27 AB -> 28 ...

**Example 1:**

**Input:** columnNumber = 1

**Output:** "A"

**Example 2:**

**Input:** columnNumber = 28

**Output:** "AB"

**Example 3:**

**Input:** columnNumber = 701

**Output:** "ZY"

**Constraints:**

*   <code>1 <= columnNumber <= 2<sup>31</sup> - 1</code>

## Solution

```kotlin
class Solution {
    fun convertToTitle(columnNumber: Int): String {
        var num = columnNumber
        val sb = StringBuilder()
        while (num != 0) {
            var remainder = num % 26
            if (remainder == 0) {
                remainder += 26
            }
            if (num >= remainder) {
                num -= remainder
                sb.append((remainder + 64).toChar())
            }
            num /= 26
        }
        return sb.reverse().toString()
    }
}
```