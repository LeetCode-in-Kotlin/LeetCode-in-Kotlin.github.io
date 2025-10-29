[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3726\. Remove Zeros in Decimal Representation

Easy

You are given a **positive** integer `n`.

Return the integer obtained by removing all zeros from the decimal representation of `n`.

**Example 1:**

**Input:** n = 1020030

**Output:** 123

**Explanation:**

After removing all zeros from 1**<ins>0</ins>**2**<ins>00</ins>**3**<ins>0</ins>**, we get 123.

**Example 2:**

**Input:** n = 1

**Output:** 1

**Explanation:**

1 has no zero in its decimal representation. Therefore, the answer is 1.

**Constraints:**

*   <code>1 <= n <= 10<sup>15</sup></code>

## Solution

```kotlin
class Solution {
    fun removeZeros(n: Long): Long {
        val x = StringBuilder()
        val s = n.toString()
        for (i in 0..<s.length) {
            if (s[i] != '0') {
                x.append(s[i])
            }
        }
        return x.toString().toLong()
    }
}
```