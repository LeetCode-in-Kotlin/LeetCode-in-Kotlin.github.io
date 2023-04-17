[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 918\. Maximum Sum Circular Subarray

Medium

Given a **circular integer array** `nums` of length `n`, return _the maximum possible sum of a non-empty **subarray** of_ `nums`.

A **circular array** means the end of the array connects to the beginning of the array. Formally, the next element of `nums[i]` is `nums[(i + 1) % n]` and the previous element of `nums[i]` is `nums[(i - 1 + n) % n]`.

A **subarray** may only include each element of the fixed buffer `nums` at most once. Formally, for a subarray `nums[i], nums[i + 1], ..., nums[j]`, there does not exist `i <= k1`, `k2 <= j` with `k1 % n == k2 % n`.

**Example 1:**

**Input:** nums = [1,-2,3,-2]

**Output:** 3

**Explanation:** Subarray [3] has maximum sum 3.

**Example 2:**

**Input:** nums = [5,-3,5]

**Output:** 10

**Explanation:** Subarray [5,5] has maximum sum 5 + 5 = 10.

**Example 3:**

**Input:** nums = [-3,-2,-3]

**Output:** -2

**Explanation:** Subarray [-2] has maximum sum -2.

**Constraints:**

*   `n == nums.length`
*   <code>1 <= n <= 3 * 10<sup>4</sup></code>
*   <code>-3 * 10<sup>4</sup> <= nums[i] <= 3 * 10<sup>4</sup></code>

## Solution

```kotlin
class Solution {
    private fun kadane(nums: IntArray, sign: Int): Int {
        var currSum = Int.MIN_VALUE
        var maxSum = Int.MIN_VALUE
        for (i in nums) {
            currSum = sign * i + currSum.coerceAtLeast(0)
            maxSum = maxSum.coerceAtLeast(currSum)
        }
        return maxSum
    }

    fun maxSubarraySumCircular(nums: IntArray): Int {
        if (nums.size == 1) {
            return nums[0]
        }
        var sumOfArray = 0
        for (i in nums) {
            sumOfArray += i
        }
        val maxSumSubarray = kadane(nums, 1)
        val minSumSubarray = kadane(nums, -1) * -1
        return if (sumOfArray == minSumSubarray) {
            maxSumSubarray
        } else {
            maxSumSubarray.coerceAtLeast(sumOfArray - minSumSubarray)
        }
    }
}
```