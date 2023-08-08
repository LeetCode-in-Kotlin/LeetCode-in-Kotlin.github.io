[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2749\. Minimum Operations to Make the Integer Zero

Medium

You are given two integers `num1` and `num2`.

In one operation, you can choose integer `i` in the range `[0, 60]` and subtract <code>2<sup>i</sup> + num2</code> from `num1`.

Return _the integer denoting the **minimum** number of operations needed to make_ `num1` _equal to_ `0`.

If it is impossible to make `num1` equal to `0`, return `-1`.

**Example 1:**

**Input:** num1 = 3, num2 = -2

**Output:** 3

**Explanation:** We can make 3 equal to 0 with the following operations: 
- We choose i = 2 and substract 2<sup>2</sup> + (-2) from 3, 3 - (4 + (-2)) = 1. 
- We choose i = 2 and substract 2<sup>2</sup> + (-2) from 1, 1 - (4 + (-2)) = -1. 
- We choose i = 0 and substract 2<sup>0</sup> + (-2) from -1, (-1) - (1 + (-2)) = 0. 

It can be proven, that 3 is the minimum number of operations that we need to perform.

**Example 2:**

**Input:** num1 = 5, num2 = 7

**Output:** -1

**Explanation:** It can be proven, that it is impossible to make 5 equal to 0 with the given operation.

**Constraints:**

*   <code>1 <= num1 <= 10<sup>9</sup></code>
*   <code>-10<sup>9</sup> <= num2 <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun makeTheIntegerZero(num1: Int, num2: Int): Int {
        val n1 = num1.toLong()
        val n2 = num2.toLong()
        for (i in 0..60) {
            val target = n1 - n2 * i
            val noOfBits = java.lang.Long.bitCount(target)
            if (i.toLong() in noOfBits..target) {
                return i
            }
        }
        return -1
    }
}
```