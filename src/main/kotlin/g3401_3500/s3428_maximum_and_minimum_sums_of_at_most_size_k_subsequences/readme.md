[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3428\. Maximum and Minimum Sums of at Most Size K Subsequences

Medium

You are given an integer array `nums` and a positive integer `k`. Return the sum of the **maximum** and **minimum** elements of all

**subsequences**

of `nums` with **at most** `k` elements.

Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** nums = [1,2,3], k = 2

**Output:** 24

**Explanation:**

The subsequences of `nums` with at most 2 elements are:

| **Subsequence** | Minimum | Maximum | Sum  |
|-----------------|---------|---------|------|
| `[1]`           | 1       | 1       | 2    |
| `[2]`           | 2       | 2       | 4    |
| `[3]`           | 3       | 3       | 6    |
| `[1, 2]`        | 1       | 2       | 3    |
| `[1, 3]`        | 1       | 3       | 4    |
| `[2, 3]`        | 2       | 3       | 5    |
| **Final Total** |         |         | 24   |

The output would be 24.

**Example 2:**

**Input:** nums = [5,0,6], k = 1

**Output:** 22

**Explanation:**

For subsequences with exactly 1 element, the minimum and maximum values are the element itself. Therefore, the total is `5 + 5 + 0 + 0 + 6 + 6 = 22`.

**Example 3:**

**Input:** nums = [1,1,1], k = 2

**Output:** 12

**Explanation:**

The subsequences `[1, 1]` and `[1]` each appear 3 times. For all of them, the minimum and maximum are both 1. Thus, the total is 12.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>0 <= nums[i] <= 10<sup>9</sup></code>
*   `1 <= k <= min(70, nums.length)`

## Solution

```kotlin
class Solution {
    private lateinit var fact: LongArray
    private lateinit var inv: LongArray

    private fun precomputeFactorials(n: Int) {
        fact = LongArray(n + 1)
        inv = LongArray(n + 1)
        fact[0] = 1
        for (i in 1..n) {
            fact[i] = (fact[i - 1] * i) % MOD
        }
        inv[n] = power(fact[n], (MOD - 2).toLong(), MOD)
        for (i in n - 1 downTo 0) {
            inv[i] = (inv[i + 1] * (i + 1)) % MOD
        }
    }

    private fun power(a: Long, b: Long, m: Int): Long {
        var a = a
        var b = b
        var `val`: Long = 1
        a = a % m
        while (b > 0) {
            if ((b and 1L) == 1L) `val` = (`val` * a) % m
            b = b shr 1
            a = (a * a) % m
        }
        return `val`
    }

    private fun nCr(n: Int, r: Int): Long {
        if (r < 0 || r > n) return 0
        return (fact[n] * inv[r] % MOD * inv[n - r]) % MOD
    }

    fun minMaxSums(nums: IntArray, k: Int): Int {
        val n = nums.size
        nums.sort()
        precomputeFactorials(n)
        var sum: Long = 0
        for (i in 0..<n) {
            var `val`: Long = 0
            val a = i
            val b = n - 1 - i
            run {
                var j = 0
                while (j < k && j <= a) {
                    `val` = (`val` + nCr(a, j)) % MOD
                    j++
                }
            }
            sum = (sum + (`val` * nums[i]) % MOD) % MOD
            `val` = 0
            var j = 0
            while (j < k && j <= b) {
                `val` = (`val` + nCr(b, j)) % MOD
                j++
            }
            sum = (sum + (`val` * nums[i]) % MOD) % MOD
        }
        return sum.toInt()
    }

    companion object {
        private const val MOD = 1e9.toInt() + 7
    }
}
```