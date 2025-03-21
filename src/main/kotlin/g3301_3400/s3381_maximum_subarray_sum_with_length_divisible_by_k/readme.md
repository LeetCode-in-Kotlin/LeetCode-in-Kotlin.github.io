[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3381\. Maximum Subarray Sum With Length Divisible by K

Medium

You are given an array of integers `nums` and an integer `k`.

Return the **maximum** sum of a **non-empty subarray** of `nums`, such that the size of the subarray is **divisible** by `k`.

A **subarray** is a contiguous **non-empty** sequence of elements within an array.

**Example 1:**

**Input:** nums = [1,2], k = 1

**Output:** 3

**Explanation:**

The subarray `[1, 2]` with sum 3 has length equal to 2 which is divisible by 1.

**Example 2:**

**Input:** nums = [-1,-2,-3,-4,-5], k = 4

**Output:** \-10

**Explanation:**

The maximum sum subarray is `[-1, -2, -3, -4]` which has length equal to 4 which is divisible by 4.

**Example 3:**

**Input:** nums = [-5,1,2,-3,4], k = 2

**Output:** 4

**Explanation:**

The maximum sum subarray is `[1, 2, -3, 4]` which has length equal to 4 which is divisible by 2.

**Constraints:**

*   <code>1 <= k <= nums.length <= 2 * 10<sup>5</sup></code>
*   <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun maxSubarraySum(nums: IntArray, k: Int): Long {
        val n = nums.size
        val maxSum = LongArray(n)
        var minSum: Long = 0
        for (i in n - 1 downTo n - k + 1) {
            maxSum[i] = Int.Companion.MIN_VALUE.toLong()
            minSum += nums[i]
        }
        minSum += nums[n - k]
        maxSum[n - k] = minSum
        var ans = minSum
        for (i in n - k - 1 downTo 0) {
            minSum = minSum + nums[i] - nums[i + k]
            maxSum[i] = max(minSum, (minSum + maxSum[i + k]))
            ans = max(maxSum[i], ans)
        }
        return ans
    }
}
```