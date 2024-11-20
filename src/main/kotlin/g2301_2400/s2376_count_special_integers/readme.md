[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2376\. Count Special Integers

Hard

We call a positive integer **special** if all of its digits are **distinct**.

Given a **positive** integer `n`, return _the number of special integers that belong to the interval_ `[1, n]`.

**Example 1:**

**Input:** n = 20

**Output:** 19

**Explanation:** All the integers from 1 to 20, except 11, are special. Thus, there are 19 special integers. 

**Example 2:**

**Input:** n = 5

**Output:** 5

**Explanation:** All the integers from 1 to 5 are special. 

**Example 3:**

**Input:** n = 135

**Output:** 110

**Explanation:** There are 110 integers from 1 to 135 that are special.

Some of the integers that are not special are: 22, 114, and 131.

**Constraints:**

*   <code>1 <= n <= 2 * 10<sup>9</sup></code>

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    private lateinit var cntMap: IntArray

    // number n as an array, splitted by each digit
    private lateinit var digits: IntArray

    fun countSpecialNumbers(n: Int): Int {
        var n = n
        if (n < 10) {
            return n
        }
        val len = Math.log10(n.toDouble()).toInt() + 1
        cntMap = IntArray(len - 1)
        val res = countUnbounded(len)
        digits = IntArray(len)
        var i = len - 1
        while (i >= 0) {
            digits[i] = n % 10
            i--
            n /= 10
        }
        return res + dfs(0, 0)
    }

    private fun dfs(i: Int, mask: Int): Int {
        if (i == digits.size) {
            return 1
        }
        var res = 0
        val startJ = if (i == 0) 1 else 0
        for (j in startJ until digits[i]) {
            if (mask and (1 shl j) == 0) {
                // unbounded lens left
                val unbounded = digits.size - 2 - i
                res += if (unbounded >= 0) count(unbounded, 9 - i) else 1
            }
        }
        if (mask and (1 shl digits[i]) == 0) {
            res += dfs(i + 1, mask or (1 shl digits[i]))
        }
        return res
    }

    private fun count(i: Int, max: Int): Int {
        return if (i == 0) {
            max
        } else {
            (max - i) * count(i - 1, max)
        }
    }

    private fun countUnbounded(len: Int): Int {
        var res = 9
        cntMap[0] = 9
        for (i in 0 until len - 2) {
            cntMap[i + 1] = cntMap[i] * (9 - i)
            res += cntMap[i + 1]
        }
        return res
    }
}
```