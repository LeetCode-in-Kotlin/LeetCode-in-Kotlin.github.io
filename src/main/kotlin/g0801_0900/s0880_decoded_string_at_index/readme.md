[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 880\. Decoded String at Index

Medium

You are given an encoded string `s`. To decode the string to a tape, the encoded string is read one character at a time and the following steps are taken:

*   If the character read is a letter, that letter is written onto the tape.
*   If the character read is a digit `d`, the entire current tape is repeatedly written `d - 1` more times in total.

Given an integer `k`, return _the_ <code>k<sup>th</sup></code> _letter (**1-indexed)** in the decoded string_.

**Example 1:**

**Input:** s = "leet2code3", k = 10

**Output:** "o"

**Explanation:** The decoded string is "leetleetcodeleetleetcodeleetleetcode". The 10<sup>th</sup> letter in the string is "o".

**Example 2:**

**Input:** s = "ha22", k = 5

**Output:** "h"

**Explanation:** The decoded string is "hahahaha". The 5<sup>th</sup> letter is "h".

**Example 3:**

**Input:** s = "a2345678999999999999999", k = 1

**Output:** "a"

**Explanation:** The decoded string is "a" repeated 8301530446056247680 times. The 1<sup>st</sup> letter is "a".

**Constraints:**

*   `2 <= s.length <= 100`
*   `s` consists of lowercase English letters and digits `2` through `9`.
*   `s` starts with a letter.
*   <code>1 <= k <= 10<sup>9</sup></code>
*   It is guaranteed that `k` is less than or equal to the length of the decoded string.
*   The decoded string is guaranteed to have less than <code>2<sup>63</sup></code> letters.

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun decodeAtIndex(s: String, k: Int): String {
        var k = k
        var i = 0
        var count: Long = 0
        while (i < s.length && count <= k) {
            val c = s[i]
            count = if (Character.isDigit(c)) count * (c.code - '0'.code) else count + 1
            i++
        }
        i--
        while (i < s.length) {
            val c = s[i]
            if (Character.isDigit(c)) {
                count /= (c.code - '0'.code).toLong()
                k %= count.toInt()
            } else {
                if (k % count == 0L) {
                    break
                }
                --count
            }
            i--
        }
        return s[i].toString()
    }
}
```