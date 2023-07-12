[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2578\. Split With Minimum Sum

Easy

Given a positive integer `num`, split it into two non-negative integers `num1` and `num2` such that:

*   The concatenation of `num1` and `num2` is a permutation of `num`.
    *   In other words, the sum of the number of occurrences of each digit in `num1` and `num2` is equal to the number of occurrences of that digit in `num`.
*   `num1` and `num2` can contain leading zeros.

Return _the **minimum** possible sum of_ `num1` _and_ `num2`.

**Notes:**

*   It is guaranteed that `num` does not contain any leading zeros.
*   The order of occurrence of the digits in `num1` and `num2` may differ from the order of occurrence of `num`.

**Example 1:**

**Input:** num = 4325

**Output:** 59

**Explanation:** We can split 4325 so that `num1` is 24 and num2 `is` 35, giving a sum of 59. We can prove that 59 is indeed the minimal possible sum.

**Example 2:**

**Input:** num = 687

**Output:** 75

**Explanation:** We can split 687 so that `num1` is 68 and `num2` is 7, which would give an optimal sum of 75.

**Constraints:**

*   <code>10 <= num <= 10<sup>9</sup></code>

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun splitNum(num: Int): Int {
        var num = num
        val ans = IntArray(10)
        while (num > 0) {
            ans[num % 10]++
            num /= 10
        }
        var num1 = 0
        var num2 = 0
        for (i in 0..9) {
            for (j in 0 until ans[i]) {
                if (num1 <= num2) {
                    num1 = num1 * 10 + i
                } else {
                    num2 = num2 * 10 + i
                }
            }
        }
        return num1 + num2
    }
}
```