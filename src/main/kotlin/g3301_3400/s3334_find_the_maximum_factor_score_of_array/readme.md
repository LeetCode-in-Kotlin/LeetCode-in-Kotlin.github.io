[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3334\. Find the Maximum Factor Score of Array

Medium

You are given an integer array `nums`.

The **factor score** of an array is defined as the _product_ of the LCM and GCD of all elements of that array.

Return the **maximum factor score** of `nums` after removing **at most** one element from it.

**Note** that _both_ the LCM and GCD of a single number are the number itself, and the _factor score_ of an **empty** array is 0.

The term `lcm(a, b)` denotes the **least common multiple** of `a` and `b`.

The term `gcd(a, b)` denotes the **greatest common divisor** of `a` and `b`.

**Example 1:**

**Input:** nums = [2,4,8,16]

**Output:** 64

**Explanation:**

On removing 2, the GCD of the rest of the elements is 4 while the LCM is 16, which gives a maximum factor score of `4 * 16 = 64`.

**Example 2:**

**Input:** nums = [1,2,3,4,5]

**Output:** 60

**Explanation:**

The maximum factor score of 60 can be obtained without removing any elements.

**Example 3:**

**Input:** nums = [3]

**Output:** 9

**Constraints:**

*   `1 <= nums.length <= 100`
*   `1 <= nums[i] <= 30`

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun maxScore(nums: IntArray): Long {
        val n = nums.size
        if (n == 1) {
            return nums[0].toLong() * nums[0]
        }
        val lToR = Array<LongArray?>(n) { LongArray(2) }
        val rToL = Array<LongArray?>(n) { LongArray(2) }
        for (i in 0 until n) {
            if (i == 0) {
                lToR[i]!![1] = nums[i].toLong()
                lToR[i]!![0] = lToR[i]!![1]
                rToL[n - i - 1]!![1] = nums[n - i - 1].toLong()
                rToL[n - i - 1]!![0] = rToL[n - i - 1]!![1]
            } else {
                rToL[n - i - 1]!![0] = gcd(nums[n - i - 1].toLong(), rToL[n - i]!![0])
                lToR[i]!![0] = gcd(nums[i].toLong(), lToR[i - 1]!![0])

                rToL[n - i - 1]!![1] = lcm(nums[n - i - 1].toLong(), rToL[n - i]!![1])
                lToR[i]!![1] = lcm(nums[i].toLong(), lToR[i - 1]!![1])
            }
        }
        var max: Long = 0
        for (i in 0 until n) {
            val gcd = if (i == 0) rToL[i + 1]!![0] else getLong(i, n, lToR, rToL)
            max = max(max, (gcd * (if (i == 0) rToL[i + 1]!![1] else getaLong(i, n, lToR, rToL))))
        }
        return max(max, (rToL[0]!![0] * rToL[0]!![1]))
    }

    private fun getaLong(i: Int, n: Int, lToR: Array<LongArray?>, rToL: Array<LongArray?>): Long {
        return if (i == n - 1) lToR[i - 1]!![1] else lcm(rToL[i + 1]!![1], lToR[i - 1]!![1])
    }

    private fun getLong(i: Int, n: Int, lToR: Array<LongArray?>, rToL: Array<LongArray?>): Long {
        return if (i == n - 1) lToR[i - 1]!![0] else gcd(rToL[i + 1]!![0], lToR[i - 1]!![0])
    }

    private fun gcd(a: Long, b: Long): Long {
        if (b == 0L) {
            return a
        }
        return gcd(b, a % b)
    }

    private fun lcm(a: Long, b: Long): Long {
        return a * b / gcd(a, b)
    }
}
```