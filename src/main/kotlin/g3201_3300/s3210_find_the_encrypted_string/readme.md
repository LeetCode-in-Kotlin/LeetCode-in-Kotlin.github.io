[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3210\. Find the Encrypted String

Easy

You are given a string `s` and an integer `k`. Encrypt the string using the following algorithm:

*   For each character `c` in `s`, replace `c` with the <code>k<sup>th</sup></code> character after `c` in the string (in a cyclic manner).

Return the _encrypted string_.

**Example 1:**

**Input:** s = "dart", k = 3

**Output:** "tdar"

**Explanation:**

*   For `i = 0`, the 3<sup>rd</sup> character after `'d'` is `'t'`.
*   For `i = 1`, the 3<sup>rd</sup> character after `'a'` is `'d'`.
*   For `i = 2`, the 3<sup>rd</sup> character after `'r'` is `'a'`.
*   For `i = 3`, the 3<sup>rd</sup> character after `'t'` is `'r'`.

**Example 2:**

**Input:** s = "aaa", k = 1

**Output:** "aaa"

**Explanation:**

As all the characters are the same, the encrypted string will also be the same.

**Constraints:**

*   `1 <= s.length <= 100`
*   <code>1 <= k <= 10<sup>4</sup></code>
*   `s` consists only of lowercase English letters.

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun getEncryptedString(s: String, k: Int): String {
        var k = k
        val n = s.length
        k %= n
        val str = StringBuilder(s.substring(k, n))
        str.append(s.substring(0, k))
        return str.toString()
    }
}
```