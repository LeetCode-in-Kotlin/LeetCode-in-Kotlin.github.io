[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1017\. Convert to Base -2

Medium

Given an integer `n`, return _a binary string representing its representation in base_ `-2`.

**Note** that the returned string should not have leading zeros unless the string is `"0"`.

**Example 1:**

**Input:** n = 2

**Output:** "110" **Explantion:** (-2)<sup>2</sup> + (-2)<sup>1</sup> = 2

**Example 2:**

**Input:** n = 3

**Output:** "111" **Explantion:** (-2)<sup>2</sup> + (-2)<sup>1</sup> + (-2)<sup>0</sup> = 3

**Example 3:**

**Input:** n = 4

**Output:** "100" **Explantion:** (-2)<sup>2</sup> = 4

**Constraints:**

*   <code>0 <= n <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun baseNeg2(n: Int): String {
        val sb = StringBuilder(Integer.toBinaryString(n))
        sb.reverse()
        var carry = 0
        var sum: Int
        var pos = 0
        while (pos < sb.length) {
            sum = carry + sb[pos].code - '0'.code
            sb.setCharAt(pos, if (sum % 2 == 0) '0' else '1')
            carry = sum / 2
            if (pos % 2 == 1 && sb[pos] == '1') {
                carry += 1
            }
            pos++
            if (pos >= sb.length && carry > 0) {
                sb.append(Integer.toBinaryString(carry))
                carry = 0
            }
        }
        return sb.reverse().toString()
    }
}
```