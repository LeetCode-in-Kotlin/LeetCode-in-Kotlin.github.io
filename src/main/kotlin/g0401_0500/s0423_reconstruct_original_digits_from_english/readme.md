[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 423\. Reconstruct Original Digits from English

Medium

Given a string `s` containing an out-of-order English representation of digits `0-9`, return _the digits in **ascending** order_.

**Example 1:**

**Input:** s = "owoztneoer"

**Output:** "012"

**Example 2:**

**Input:** s = "fviefuro"

**Output:** "45"

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s[i]` is one of the characters `["e","g","f","i","h","o","n","s","r","u","t","w","v","x","z"]`.
*   `s` is **guaranteed** to be valid.

## Solution

```kotlin
class Solution {
    fun originalDigits(s: String): String {
        val count = IntArray(26)
        val digits = IntArray(10)
        val str = StringBuilder()
        for (c in s.toCharArray()) {
            ++count[c.code - 'a'.code]
        }
        digits[0] = count[25]
        digits[2] = count[22]
        digits[4] = count[20]
        digits[6] = count[23]
        digits[8] = count[6]
        digits[1] = count[14] - digits[0] - digits[2] - digits[4]
        digits[3] = count[7] - digits[8]
        digits[5] = count[5] - digits[4]
        digits[7] = count[18] - digits[6]
        digits[9] = count[8] - digits[5] - digits[6] - digits[8]
        for (i in 0..9) {
            while (digits[i]-- != 0) {
                str.append((i + 48).toChar())
            }
        }
        return str.toString()
    }
}
```