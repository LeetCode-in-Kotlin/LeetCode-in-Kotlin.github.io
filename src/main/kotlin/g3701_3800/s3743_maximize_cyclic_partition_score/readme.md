[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3743\. Maximize Cyclic Partition Score

Hard

You are given a **cyclic** array `nums` and an integer `k`.

**Partition** `nums` into **at most** `k` non-empty subarrays. As `nums` is cyclic, these subarrays may wrap around from the end of the array back to the beginning.

The **range** of a subarray is the difference between its **maximum** and **minimum** values. The **score** of a partition is the sum of subarray **ranges**.

Return the **maximum** possible **score** among all cyclic partitions.

**Example 1:**

**Input:** nums = [1,2,3,3], k = 2

**Output:** 3

**Explanation:**

*   Partition `nums` into `[2, 3]` and `[3, 1]` (wrapped around).
*   The range of `[2, 3]` is `max(2, 3) - min(2, 3) = 3 - 2 = 1`.
*   The range of `[3, 1]` is `max(3, 1) - min(3, 1) = 3 - 1 = 2`.
*   The score is `1 + 2 = 3`.

**Example 2:**

**Input:** nums = [1,2,3,3], k = 1

**Output:** 2

**Explanation:**

*   Partition `nums` into `[1, 2, 3, 3]`.
*   The range of `[1, 2, 3, 3]` is `max(1, 2, 3, 3) - min(1, 2, 3, 3) = 3 - 1 = 2`.
*   The score is 2.

**Example 3:**

**Input:** nums = [1,2,3,3], k = 4

**Output:** 3

**Explanation:**

Identical to Example 1, we partition `nums` into `[2, 3]` and `[3, 1]`. Note that `nums` may be partitioned into fewer than `k` subarrays.

**Constraints:**

*   `1 <= nums.length <= 1000`
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   `1 <= k <= nums.length`

## Solution

```kotlin
import kotlin.math.max
import kotlin.math.min

class Solution {
    fun maximumScore(nums: IntArray, k: Int): Long {
        // Find index of minimum element
        var j = 0
        for (i in nums.indices) {
            if (nums[i] < nums[j]) {
                j = i
            }
        }
        // Build array 'a'
        val n = nums.size
        val a = IntArray(n)
        for (i in 0..<n) {
            a[i] = nums[(j + i) % n]
        }
        // Build array 'b' (rotated and reversed)
        val b = IntArray(n)
        for (i in 0..<n) {
            b[i] = nums[(j + 1 + i) % n]
        }
        reverse(b)
        // Compute and return max of f(a, k) and f(b, k)
        return max(f(a, k), f(b, k))
    }

    private fun f(a: IntArray, k: Int): Long {
        val n = a.size
        val dp = Array<LongArray?>(k + 1) { LongArray(n + 1) }
        var mn = Long.MAX_VALUE
        var mx = Long.MIN_VALUE
        // Initialize dp[1][j+1]
        for (j in 0..<n) {
            mn = min(mn, a[j].toLong())
            mx = max(mx, a[j].toLong())
            dp[1]!![j + 1] = mx - mn
        }
        var res = dp[1]!![n]
        for (i in 2..k) {
            var x = Long.MIN_VALUE
            var y = Long.MIN_VALUE
            for (j in i - 1..<n) {
                x = max(x, dp[i - 1]!![j] - a[j])
                y = max(y, dp[i - 1]!![j] + a[j])
                dp[i]!![j + 1] = max(dp[i]!![j], max(x + a[j], y - a[j]))
            }
            res = max(res, dp[i]!![n])
        }
        return res
    }

    private fun reverse(arr: IntArray) {
        var left = 0
        var right = arr.size - 1
        while (left < right) {
            val temp = arr[left]
            arr[left] = arr[right]
            arr[right] = temp
            left++
            right--
        }
    }
}
```