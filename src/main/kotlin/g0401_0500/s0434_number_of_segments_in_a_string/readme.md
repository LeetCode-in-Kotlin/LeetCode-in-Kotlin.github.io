[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 434\. Number of Segments in a String

Easy

Given a string `s`, return _the number of segments in the string_.

A **segment** is defined to be a contiguous sequence of **non-space characters**.

**Example 1:**

**Input:** s = "Hello, my name is John"

**Output:** 5

**Explanation:** The five segments are ["Hello,", "my", "name", "is", "John"]

**Example 2:**

**Input:** s = "Hello"

**Output:** 1

**Constraints:**

*   `0 <= s.length <= 300`
*   `s` consists of lowercase and uppercase English letters, digits, or one of the following characters `"!@#$%^&*()_+-=',.:"`.
*   The only space character in `s` is `' '`.

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun countSegments(s: String): Int {
        var s = s
        s = s.trim { it <= ' ' }
        if (s.length == 0) {
            return 0
        }
        val splitted = s.split(" ".toRegex()).dropLastWhile { it.isEmpty() }.toTypedArray()
        var result = 0
        for (value in splitted) {
            if (value.length > 0) {
                result++
            }
        }
        return result
    }
}
```