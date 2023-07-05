[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2429\. Minimize XOR

Medium

Given two positive integers `num1` and `num2`, find the positive integer `x` such that:

*   `x` has the same number of set bits as `num2`, and
*   The value `x XOR num1` is **minimal**.

Note that `XOR` is the bitwise XOR operation.

Return _the integer_ `x`. The test cases are generated such that `x` is **uniquely determined**.

The number of **set bits** of an integer is the number of `1`'s in its binary representation.

**Example 1:**

**Input:** num1 = 3, num2 = 5

**Output:** 3

**Explanation:** The binary representations of num1 and num2 are 0011 and 0101, respectively. The integer **3** has the same number of set bits as num2, and the value `3 XOR 3 = 0` is minimal.

**Example 2:**

**Input:** num1 = 1, num2 = 12

**Output:** 3

**Explanation:** The binary representations of num1 and num2 are 0001 and 1100, respectively. The integer **3** has the same number of set bits as num2, and the value `3 XOR 1 = 2` is minimal.

**Constraints:**

*   <code>1 <= num1, num2 <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun minimizeXor(num1: Int, num2: Int): Int {
        var bits = Integer.bitCount(num2)
        var result = 0
        run {
            var i = 30
            while (i >= 0 && bits > 0) {
                if (1 shl i and num1 != 0) {
                    --bits
                    result = result xor (1 shl i)
                }
                --i
            }
        }
        var i = 0
        while (i <= 30 && bits > 0) {
            if (1 shl i and num1 == 0) {
                --bits
                result = result xor (1 shl i)
            }
            ++i
        }
        return result
    }
}
```