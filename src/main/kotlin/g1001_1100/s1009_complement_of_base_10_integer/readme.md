[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1009\. Complement of Base 10 Integer

Easy

The **complement** of an integer is the integer you get when you flip all the `0`'s to `1`'s and all the `1`'s to `0`'s in its binary representation.

*   For example, The integer `5` is `"101"` in binary and its **complement** is `"010"` which is the integer `2`.

Given an integer `n`, return _its complement_.

**Example 1:**

**Input:** n = 5

**Output:** 2

**Explanation:** 5 is "101" in binary, with complement "010" in binary, which is 2 in base-10.

**Example 2:**

**Input:** n = 7

**Output:** 0

**Explanation:** 7 is "111" in binary, with complement "000" in binary, which is 0 in base-10.

**Example 3:**

**Input:** n = 10

**Output:** 5

**Explanation:** 10 is "1010" in binary, with complement "0101" in binary, which is 5 in base-10.

**Constraints:**

*   <code>0 <= n < 10<sup>9</sup></code>

**Note:** This question is the same as 476: [https://leetcode.com/problems/number-complement/](https://leetcode.com/problems/number-complement/)

## Solution

```kotlin
import kotlin.math.pow

@Suppress("NAME_SHADOWING")
class Solution {
    fun bitwiseComplement(n: Int): Int {
        var n = n
        if (n == 0) {
            return 1
        }
        val list: MutableList<Int> = ArrayList()
        while (n != 0) {
            list.add(n and 1)
            n = n shr 1
        }
        var result = 0
        var exp = list.size - 1
        for (i in list.indices.reversed()) {
            if (list[i] == 0) {
                result += 2.0.pow(exp.toDouble()).toInt()
            }
            exp--
        }
        return result
    }
}
```