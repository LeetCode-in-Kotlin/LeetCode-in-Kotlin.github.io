[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 67\. Add Binary

Easy

Given two binary strings `a` and `b`, return _their sum as a binary string_.

**Example 1:**

**Input:** a = "11", b = "1"

**Output:** "100"

**Example 2:**

**Input:** a = "1010", b = "1011"

**Output:** "10101"

**Constraints:**

*   <code>1 <= a.length, b.length <= 10<sup>4</sup></code>
*   `a` and `b` consist only of `'0'` or `'1'` characters.
*   Each string does not contain leading zeros except for the zero itself.

## Solution

```kotlin
class Solution {
    fun addBinary(a: String, b: String): String {
        val aArray = a.toCharArray()
        val bArray = b.toCharArray()
        val sb = StringBuilder()
        var i = aArray.size - 1
        var j = bArray.size - 1
        var carry = 0
        while (i >= 0 || j >= 0) {
            val sum = (if (i >= 0) aArray[i] - '0' else 0) + (if (j >= 0) bArray[j] - '0' else 0) + carry
            sb.append(sum % 2)
            carry = sum / 2
            if (i >= 0) {
                i--
            }
            if (j >= 0) {
                j--
            }
        }
        if (carry != 0) {
            sb.append(carry)
        }
        return sb.reverse().toString()
    }
}
```