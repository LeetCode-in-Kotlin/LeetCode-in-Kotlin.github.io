[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2165\. Smallest Value of the Rearranged Number

Medium

You are given an integer `num.` **Rearrange** the digits of `num` such that its value is **minimized** and it does not contain **any** leading zeros.

Return _the rearranged number with minimal value_.

Note that the sign of the number does not change after rearranging the digits.

**Example 1:**

**Input:** num = 310

**Output:** 103

**Explanation:** The possible arrangements for the digits of 310 are 013, 031, 103, 130, 301, 310. 

The arrangement with the smallest value that does not contain any leading zeros is 103. 

**Example 2:**

**Input:** num = -7605

**Output:** -7650

**Explanation:** Some possible arrangements for the digits of -7605 are -7650, -6705, -5076, -0567. 

The arrangement with the smallest value that does not contain any leading zeros is -7650. 

**Constraints:**

*   <code>-10<sup>15</sup> <= num <= 10<sup>15</sup></code>

## Solution

```kotlin
class Solution {
    fun smallestNumber(num: Long): Long {
        val count = IntArray(10)
        var tempNum: Long
        tempNum = if (num > 0) {
            num
        } else {
            num * -1
        }
        var min = 10
        while (tempNum > 0) {
            val rem = (tempNum % 10).toInt()
            if (rem != 0) {
                min = Math.min(min, rem)
            }
            count[rem]++
            tempNum = tempNum / 10
        }
        var output: Long = 0
        if (num > 0) {
            output = output * 10 + min
            count[min]--
            for (i in 0..9) {
                for (j in 0 until count[i]) {
                    output = output * 10 + i
                }
            }
        } else {
            for (i in 9 downTo 0) {
                for (j in 0 until count[i]) {
                    output = output * 10 + i
                }
            }
            output = output * -1
        }
        return output
    }
}
```