[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 709\. To Lower Case

Easy

Given a string `s`, return _the string after replacing every uppercase letter with the same lowercase letter_.

**Example 1:**

**Input:** s = "Hello"

**Output:** "hello"

**Example 2:**

**Input:** s = "here"

**Output:** "here"

**Example 3:**

**Input:** s = "LOVELY"

**Output:** "lovely"

**Constraints:**

*   `1 <= s.length <= 100`
*   `s` consists of printable ASCII characters.

## Solution

```kotlin
class Solution {
    fun toLowerCase(s: String): String {
        val c = s.toCharArray()
        for (i in s.indices) {
            if (c[i] in 'A'..'Z') {
                c[i] = (c[i].code - 'A'.code + 'a'.code).toChar()
            }
        }
        return String(c)
    }
}
```