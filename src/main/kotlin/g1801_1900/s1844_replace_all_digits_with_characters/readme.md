[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1844\. Replace All Digits with Characters

Easy

You are given a **0-indexed** string `s` that has lowercase English letters in its **even** indices and digits in its **odd** indices.

There is a function `shift(c, x)`, where `c` is a character and `x` is a digit, that returns the <code>x<sup>th</sup></code> character after `c`.

*   For example, `shift('a', 5) = 'f'` and `shift('x', 0) = 'x'`.

For every **odd** index `i`, you want to replace the digit `s[i]` with `shift(s[i-1], s[i])`.

Return `s` _after replacing all digits. It is **guaranteed** that_ `shift(s[i-1], s[i])` _will never exceed_ `'z'`.

**Example 1:**

**Input:** s = "a1c1e1"

**Output:** "abcdef"

**Explanation:** The digits are replaced as follows: 

- s[1] -> shift('a',1) = 'b' 

- s[3] -> shift('c',1) = 'd' 

- s[5] -> shift('e',1) = 'f'

**Example 2:**

**Input:** s = "a1b2c3d4e"

**Output:** "abbdcfdhe"

**Explanation:** The digits are replaced as follows: 

- s[1] -> shift('a',1) = 'b' 

- s[3] -> shift('b',2) = 'd' 

- s[5] -> shift('c',3) = 'f' 

- s[7] -> shift('d',4) = 'h'

**Constraints:**

*   `1 <= s.length <= 100`
*   `s` consists only of lowercase English letters and digits.
*   `shift(s[i-1], s[i]) <= 'z'` for all **odd** indices `i`.

## Solution

```kotlin
class Solution {
    fun replaceDigits(s: String): String {
        val sb = StringBuilder()
        for (c in s.toCharArray()) {
            if (Character.isAlphabetic(c.code)) {
                sb.append(c)
            } else {
                sb.append((sb[sb.length - 1].code + Character.getNumericValue(c)).toChar())
            }
        }
        return sb.toString()
    }
}
```