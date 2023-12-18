[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2843\. Count Symmetric Integers

Easy

You are given two positive integers `low` and `high`.

An integer `x` consisting of `2 * n` digits is **symmetric** if the sum of the first `n` digits of `x` is equal to the sum of the last `n` digits of `x`. Numbers with an odd number of digits are never symmetric.

Return _the **number of symmetric** integers in the range_ `[low, high]`.

**Example 1:**

**Input:** low = 1, high = 100

**Output:** 9

**Explanation:** There are 9 symmetric integers between 1 and 100: 11, 22, 33, 44, 55, 66, 77, 88, and 99.

**Example 2:**

**Input:** low = 1200, high = 1230

**Output:** 4

**Explanation:** There are 4 symmetric integers between 1200 and 1230: 1203, 1212, 1221, and 1230.

**Constraints:**

*   <code>1 <= low <= high <= 10<sup>4</sup></code>

## Solution

```kotlin
class Solution {
    fun countSymmetricIntegers(low: Int, high: Int): Int {
        var count = 0
        for (i in low..high) {
            if (isSymmetric(i)) {
                count++
            }
        }
        return count
    }

    private fun isSymmetric(num: Int): Boolean {
        val str = num.toString()
        val n = str.length
        if (n % 2 != 0) {
            return false
        }
        var leftSum = 0
        var rightSum = 0
        var i = 0
        var j = n - 1
        while (i < j) {
            leftSum += str[i].code - '0'.code
            rightSum += str[j].code - '0'.code
            i++
            j--
        }
        return leftSum == rightSum
    }
}
```