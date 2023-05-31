[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1031\. Maximum Sum of Two Non-Overlapping Subarrays

Medium

Given an integer array `nums` and two integers `firstLen` and `secondLen`, return _the maximum sum of elements in two non-overlapping **subarrays** with lengths_ `firstLen` _and_ `secondLen`.

The array with length `firstLen` could occur before or after the array with length `secondLen`, but they have to be non-overlapping.

A **subarray** is a **contiguous** part of an array.

**Example 1:**

**Input:** nums = [0,6,5,2,2,5,1,9,4], firstLen = 1, secondLen = 2

**Output:** 20

**Explanation:** One choice of subarrays is [9] with length 1, and [6,5] with length 2.

**Example 2:**

**Input:** nums = [3,8,1,3,2,1,8,9,0], firstLen = 3, secondLen = 2

**Output:** 29

**Explanation:** One choice of subarrays is [3,8,1] with length 3, and [8,9] with length 2.

**Example 3:**

**Input:** nums = [2,1,5,6,0,9,5,0,3,8], firstLen = 4, secondLen = 3

**Output:** 31

**Explanation:** One choice of subarrays is [5,6,0,9] with length 4, and [0,3,8] with length 3.

**Constraints:**

*   `1 <= firstLen, secondLen <= 1000`
*   `2 <= firstLen + secondLen <= 1000`
*   `firstLen + secondLen <= nums.length <= 1000`
*   `0 <= nums[i] <= 1000`

## Solution

```kotlin
class Solution {
    fun maxSumTwoNoOverlap(nums: IntArray, firstLen: Int, secondLen: Int): Int {
        val firstLenSum = getFirstLenSums(nums, firstLen)
        return getMaxLenSum(nums, secondLen, firstLenSum)
    }

    private fun getMaxLenSum(nums: IntArray, secondLen: Int, firstLenSum: Array<IntArray>): Int {
        var maxSum = 0
        var currentSum = 0
        var onRight: Int
        for (i in 0 until secondLen) {
            currentSum += nums[i]
        }
        onRight = firstLenSum[1][secondLen]
        maxSum = maxSum.coerceAtLeast(currentSum + onRight)
        var i = 1
        var j = secondLen
        while (j < nums.size) {
            currentSum = currentSum - nums[i - 1] + nums[j]
            onRight = if (j < nums.size - 1) firstLenSum[1][j + 1] else 0
            maxSum = maxSum.coerceAtLeast(currentSum + firstLenSum[0][i - 1].coerceAtLeast(onRight))
            i++
            j++
        }
        return maxSum
    }

    private fun getFirstLenSums(nums: IntArray, windowSize: Int): Array<IntArray> {
        // sum[0] - maximum from left to right, sum[1] - max from right to left.
        val sum = Array(2) { IntArray(nums.size) }
        var currentLeftSum = 0
        var currentRightSum = 0
        run {
            var i = 0
            var j = nums.size - 1
            while (i < windowSize) {
                currentLeftSum += nums[i]
                currentRightSum += nums[j]
                i++
                j--
            }
        }
        sum[0][windowSize - 1] = currentLeftSum
        sum[1][nums.size - windowSize] = currentRightSum
        var i = windowSize
        var j = nums.size - windowSize - 1
        while (i < nums.size) {
            currentLeftSum = currentLeftSum - nums[i - windowSize] + nums[i]
            currentRightSum = currentRightSum - nums[j + windowSize] + nums[j]
            sum[0][i] = sum[0][i - 1].coerceAtLeast(currentLeftSum)
            sum[1][j] = sum[1][j + 1].coerceAtLeast(currentRightSum)
            i++
            j--
        }
        return sum
    }
}
```