[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2595\. Number of Even and Odd Bits

Easy

You are given a **positive** integer `n`.

Let `even` denote the number of even indices in the binary representation of `n` (**0-indexed**) with value `1`.

Let `odd` denote the number of odd indices in the binary representation of `n` (**0-indexed**) with value `1`.

Return _an integer array_ `answer` _where_ `answer = [even, odd]`.

**Example 1:**

**Input:** n = 17

**Output:** [2,0]

**Explanation:**

The binary representation of 17 is 10001.

It contains 1 on the 0<sup>th</sup> and 4<sup>th</sup> indices.

There are 2 even and 0 odd indices.

**Example 2:**

**Input:** n = 2

**Output:** [0,1]

**Explanation:**

The binary representation of 2 is 10.

It contains 1 on the 1<sup>st</sup> index.

There are 0 even and 1 odd indices.

**Constraints:**

*   `1 <= n <= 1000`

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun evenOddBit(n: Int): IntArray {
        var n = n
        var flag = 1
        var even = 0
        var odd = 0
        while (n != 0) {
            val bit = n and 1
            if (bit == 1 && flag == 1) {
                even++
            }
            if (bit == 1 && flag != 1) {
                odd++
            }
            flag = Math.abs(flag - 1)
            n = n ushr 1
        }
        return intArrayOf(even, odd)
    }
}
```