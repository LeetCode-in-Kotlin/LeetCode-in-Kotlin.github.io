[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1556\. Thousand Separator

Easy

Given an integer `n`, add a dot (".") as the thousands separator and return it in string format.

**Example 1:**

**Input:** n = 987

**Output:** "987"

**Example 2:**

**Input:** n = 1234

**Output:** "1.234"

**Constraints:**

*   <code>0 <= n <= 2<sup>31</sup> - 1</code>

## Solution

```kotlin
class Solution {
    fun thousandSeparator(n: Int): String {
        val str = n.toString()
        val sb = StringBuilder()
        var i = str.length - 1
        var j = 1
        while (i >= 0) {
            sb.append(str[i])
            j++
            if (j % 3 == 0) {
                sb.append(".")
            }
            i--
            j++
        }
        var result = sb.reverse().toString()
        if (result[0] == '.') {
            result = result.substring(1)
        }
        return result
    }
}
```