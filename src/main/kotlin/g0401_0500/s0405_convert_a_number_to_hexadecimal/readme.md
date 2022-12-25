[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 405\. Convert a Number to Hexadecimal

Easy

Given an integer `num`, return _a string representing its hexadecimal representation_. For negative integers, [two’s complement](https://en.wikipedia.org/wiki/Two%27s_complement) method is used.

All the letters in the answer string should be lowercase characters, and there should not be any leading zeros in the answer except for the zero itself.

**Note:** You are not allowed to use any built-in library method to directly solve this problem.

**Example 1:**

**Input:** num = 26

**Output:** "1a"

**Example 2:**

**Input:** num = -1

**Output:** "ffffffff"

**Constraints:**

*   <code>-2<sup>31</sup> <= num <= 2<sup>31</sup> - 1</code>

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun toHex(num: Int): String {
        var num = num
        if (num == 0) {
            return "0"
        }
        val sb = StringBuilder()
        var x: Int
        while (num != 0) {
            x = num and 0xf
            if (x < 10) {
                sb.append(x)
            } else {
                sb.append((x + 87).toChar())
            }
            num = num ushr 4
        }
        return sb.reverse().toString()
    }
}
```