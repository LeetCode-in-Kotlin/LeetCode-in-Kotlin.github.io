[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2930\. Number of Strings Which Can Be Rearranged to Contain Substring

Medium

You are given an integer `n`.

A string `s` is called **good** if it contains only lowercase English characters **and** it is possible to rearrange the characters of `s` such that the new string contains `"leet"` as a **substring**.

For example:

*   The string `"lteer"` is good because we can rearrange it to form `"leetr"` .
*   `"letl"` is not good because we cannot rearrange it to contain `"leet"` as a substring.

Return _the **total** number of good strings of length_ `n`.

Since the answer may be large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

A **substring** is a contiguous sequence of characters within a string.

**Example 1:**

**Input:** n = 4

**Output:** 12

**Explanation:** The 12 strings which can be rearranged to have "leet" as a substring are: "eelt", "eetl", "elet", "elte", "etel", "etle", "leet", "lete", "ltee", "teel", "tele", and "tlee".

**Example 2:**

**Input:** n = 10

**Output:** 83943898

**Explanation:** The number of strings with length 10 which can be rearranged to have "leet" as a substring is 526083947580. Hence the answer is 526083947580 % (10<sup>9</sup> + 7) = 83943898.

**Constraints:**

*   <code>1 <= n <= 10<sup>5</sup></code>

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    private fun pow(x: Long, n: Long, mod: Long): Long {
        var n = n
        var result: Long = 1
        var p = x % mod
        while (n != 0L) {
            if ((n and 1L) != 0L) {
                result = (result * p) % mod
            }
            p = (p * p) % mod
            n = n shr 1
        }
        return result
    }

    fun stringCount(n: Int): Int {
        val mod = 1e9.toInt() + 7L
        return (
            (
                (
                    pow(26, n.toLong(), mod) -
                        (n + 75) * pow(25, n - 1L, mod) +
                        (2 * n + 72) * pow(24, n - 1L, mod) -
                        (n + 23) * pow(23, n - 1L, mod)
                    ) %
                    mod +
                    mod
                ) %
                mod
            ).toInt()
    }
}
```