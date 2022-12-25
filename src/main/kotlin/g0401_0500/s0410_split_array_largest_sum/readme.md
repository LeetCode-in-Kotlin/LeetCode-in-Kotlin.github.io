[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 410\. Split Array Largest Sum

Hard

Given an integer array `nums` and an integer `k`, split `nums` into `k` non-empty subarrays such that the largest sum of any subarray is **minimized**.

Return _the minimized largest sum of the split_.

A **subarray** is a contiguous part of the array.

**Example 1:**

**Input:** nums = [7,2,5,10,8], k = 2

**Output:** 18

**Explanation:** There are four ways to split nums into two subarrays. The best way is to split it into [7,2,5] and [10,8], where the largest sum among the two subarrays is only 18.

**Example 2:**

**Input:** nums = [1,2,3,4,5], k = 2

**Output:** 9

**Explanation:** There are four ways to split nums into two subarrays. The best way is to split it into [1,2,3] and [4,5], where the largest sum among the two subarrays is only 9.

**Constraints:**

*   `1 <= nums.length <= 1000`
*   <code>0 <= nums[i] <= 10<sup>6</sup></code>
*   `1 <= k <= min(50, nums.length)`

## Solution

```kotlin
class Solution {
    fun splitArray(nums: IntArray, m: Int): Int {
        var maxVal = 0
        var minVal = nums[0]
        for (num in nums) {
            maxVal += num
            minVal = Math.max(minVal, num)
        }
        while (minVal < maxVal) {
            val midVal = minVal + (maxVal - minVal) / 2
            // if we can split, try to reduce the midVal so decrease maxVal
            if (canSplit(midVal, nums, m)) {
                maxVal = midVal
            } else {
                // if we can't split, then try to increase midVal by increasing minVal
                minVal = midVal + 1
            }
        }
        return minVal
    }

    private fun canSplit(maxSubArrSum: Int, nums: IntArray, m: Int): Boolean {
        var currSum = 0
        var currSplits = 1
        for (num in nums) {
            currSum += num
            if (currSum > maxSubArrSum) {
                currSum = num
                currSplits++
                // if maxSubArrSum was TOO high that we can split the array into more that 'm' parts
                // then its not ideal
                if (currSplits > m) {
                    return false
                }
            }
        }
        return true
    }
}
```