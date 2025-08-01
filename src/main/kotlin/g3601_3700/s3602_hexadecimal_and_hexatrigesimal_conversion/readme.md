[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3602\. Hexadecimal and Hexatrigesimal Conversion

Easy

You are given an integer `n`.

Return the concatenation of the **hexadecimal** representation of <code>n<sup>2</sup></code> and the **hexatrigesimal** representation of <code>n<sup>3</sup></code>.

A **hexadecimal** number is defined as a base-16 numeral system that uses the digits `0 – 9` and the uppercase letters `A - F` to represent values from 0 to 15.

A **hexatrigesimal** number is defined as a base-36 numeral system that uses the digits `0 – 9` and the uppercase letters `A - Z` to represent values from 0 to 35.

**Example 1:**

**Input:** n = 13

**Output:** "A91P1"

**Explanation:**

*   <code>n<sup>2</sup> = 13 * 13 = 169</code>. In hexadecimal, it converts to `(10 * 16) + 9 = 169`, which corresponds to `"A9"`.
*   <code>n<sup>3</sup> = 13 * 13 * 13 = 2197</code>. In hexatrigesimal, it converts to <code>(1 * 36<sup>2</sup>) + (25 * 36) + 1 = 2197</code>, which corresponds to `"1P1"`.
*   Concatenating both results gives `"A9" + "1P1" = "A91P1"`.

**Example 2:**

**Input:** n = 36

**Output:** "5101000"

**Explanation:**

*   <code>n<sup>2</sup> = 36 * 36 = 1296</code>. In hexadecimal, it converts to <code>(5 * 16<sup>2</sup>) + (1 * 16) + 0 = 1296</code>, which corresponds to `"510"`.
*   <code>n<sup>3</sup> = 36 * 36 * 36 = 46656</code>. In hexatrigesimal, it converts to <code>(1 * 36<sup>3</sup>) + (0 * 36<sup>2</sup>) + (0 * 36) + 0 = 46656</code>, which corresponds to `"1000"`.
*   Concatenating both results gives `"510" + "1000" = "5101000"`.

**Constraints:**

*   `1 <= n <= 1000`

## Solution

```kotlin
class Solution {
    fun concatHex36(n: Int): String {
        var t = n * n
        var k: Int
        val st = StringBuilder()
        var tmp = StringBuilder()
        while (t > 0) {
            k = t % 16
            t = t / 16
            if (k <= 9) {
                tmp.append(('0'.code + k).toChar())
            } else {
                tmp.append(('A'.code + (k - 10)).toChar())
            }
        }
        for (i in tmp.length - 1 downTo 0) {
            st.append(tmp[i])
        }
        tmp = StringBuilder()
        t = n * n * n
        while (t > 0) {
            k = t % 36
            t = t / 36
            if (k <= 9) {
                tmp.append(('0'.code + k).toChar())
            } else {
                tmp.append(('A'.code + (k - 10)).toChar())
            }
        }
        for (i in tmp.length - 1 downTo 0) {
            st.append(tmp[i])
        }
        return st.toString()
    }
}
```