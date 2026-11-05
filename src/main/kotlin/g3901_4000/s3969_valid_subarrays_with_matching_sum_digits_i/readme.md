[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3969\. Valid Subarrays With Matching Sum Digits I

Medium

You are given an integer array `nums` and an integer digit `x`.

A non-empty subarrays `nums[l..r]` is considered **valid** if the sum of its elements satisfies both of the following conditions:

*   The first digit of the sum is equal to `x`.
*   The last digit of the sum is equal to `x`.

Return the number of valid subarrays.

**Example 1:**

**Input:** nums = [1,100,1], x = 1

**Output:** 4

**Explanation:**

The valid subarrays are:

*   `nums[0..0]`: `sum = 1`
*   `nums[0..1]`: `sum = 1 + 100 = 101`
*   `nums[1..2]`: `sum = 100 + 1 = 101`
*   `nums[2..2]`: `sum = 1`

Thus, the answer is 4.

**Example 2:**

**Input:** nums = [1], x = 2

**Output:** 0

**Explanation:**

The only subarray is `nums[0..0]` with a sum of 1, which does not satisfy the conditions.

Thus, the answer is 0.

**Constraints:**

*   `1 <= nums.length <= 1500`
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   `1 <= x <= 9`

## Solution

```kotlin
class Solution {
    fun countValidSubarrays(nums: IntArray, x: Int): Int {
        val n = nums.size
        val pre = LongArray(n + 1)
        for (i in 0..<n) {
            pre[i + 1] = pre[i] + nums[i]
        }
        if (x == 0) {
            return 0
        }
        val interval: MutableList<LongArray> = ArrayList<LongArray>()
        var p: Long = 1
        while (p <= pre[n]) {
            interval.add(longArrayOf(p * x, p * x + p - 1))
            p *= 10
        }
        val isit: Array<MutableList<Long>> = Array<MutableList<Long>>(10) { ArrayList() }
        for (r in 0..9) {
            isit[r] = ArrayList<Long>()
        }
        var sum: Long = 0
        for (i in 0..n) {
            if (i > 0) {
                val k = pre[i]
                val r = (((k - x) % 10 + 10) % 10).toInt()
                val list: MutableList<Long> = isit[r]
                for (j in interval) {
                    val a = k - j[1]
                    val b = k - j[0]
                    if (b < 0) {
                        continue
                    }
                    val high = upper(list, b)
                    val low = lower(list, a)
                    sum += (high - low).toLong()
                }
            }
            isit[(pre[i] % 10).toInt()].add(pre[i])
        }
        return sum.toInt()
    }

    private fun lower(list: MutableList<Long>, k: Long): Int {
        var l = 0
        var h = list.size
        while (l < h) {
            val m = (l + h) ushr 1
            if (list[m] < k) {
                l = m + 1
            } else {
                h = m
            }
        }
        return l
    }

    private fun upper(list: MutableList<Long>, k: Long): Int {
        var l = 0
        var h = list.size
        while (l < h) {
            val m = (l + h) ushr 1
            if (list[m] <= k) {
                l = m + 1
            } else {
                h = m
            }
        }
        return l
    }
}
```