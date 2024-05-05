[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3133\. Minimum Array End

Medium

You are given two integers `n` and `x`. You have to construct an array of **positive** integers `nums` of size `n` where for every `0 <= i < n - 1`, `nums[i + 1]` is **greater than** `nums[i]`, and the result of the bitwise `AND` operation between all elements of `nums` is `x`.

Return the **minimum** possible value of `nums[n - 1]`.

**Example 1:**

**Input:** n = 3, x = 4

**Output:** 6

**Explanation:**

`nums` can be `[4,5,6]` and its last element is 6.

**Example 2:**

**Input:** n = 2, x = 7

**Output:** 15

**Explanation:**

`nums` can be `[7,15]` and its last element is 15.

**Constraints:**

*   <code>1 <= n, x <= 10<sup>8</sup></code>

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun minEnd(n: Int, x: Int): Long {
        var n = n
        n -= 1
        val xb = IntArray(64)
        val nb = IntArray(64)
        for (i in 0..31) {
            xb[i] = (x shr i) and 1
            nb[i] = (n shr i) and 1
        }
        var i = 0
        var j = 0
        while (i < 64) {
            if (xb[i] != 1) {
                xb[i] = nb[j++]
            }
            i++
        }
        var ans: Long = 0
        var p: Long = 1
        i = 0
        while (i < 64) {
            ans += (xb[i]) * p
            p *= 2
            i++
        }
        return ans
    }
}
```