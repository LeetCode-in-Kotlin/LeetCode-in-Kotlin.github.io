[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1680\. Concatenation of Consecutive Binary Numbers

Medium

Given an integer `n`, return _the **decimal value** of the binary string formed by concatenating the binary representations of_ `1` _to_ `n` _in order, **modulo**_ <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** n = 1

**Output:** 1

**Explanation:** "1" in binary corresponds to the decimal value 1.

**Example 2:**

**Input:** n = 3

**Output:** 27

**Explanation:** In binary, 1, 2, and 3 corresponds to "1", "10", and "11".

After concatenating them, we have "11011", which corresponds to the decimal value 27.

**Example 3:**

**Input:** n = 12

**Output:** 505379714

**Explanation:** The concatenation results in "1101110010111011110001001101010111100".

The decimal value of that is 118505380540.

After modulo 10<sup>9</sup> + 7, the result is 505379714.

**Constraints:**

*   <code>1 <= n <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    fun concatenatedBinary(n: Int): Int {
        // calculate the length of binary string
        var length = 0
        var sum: Long = 0
        for (i in 1..n) {
            if (i and i - 1 == 0) {
                length++
            }
            sum = sum shl length
            sum += i.toLong()
            if (sum > MOD) {
                sum %= MOD
            }
        }
        return (sum % MOD).toInt()
    }

    companion object {
        private const val MOD: Long = 1000000007
    }
}
```