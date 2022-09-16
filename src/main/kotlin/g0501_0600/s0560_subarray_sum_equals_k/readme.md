[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 560\. Subarray Sum Equals K

Medium

Given an array of integers `nums` and an integer `k`, return _the total number of subarrays whose sum equals to_ `k`.

A subarray is a contiguous **non-empty** sequence of elements within an array.

**Example 1:**

**Input:** nums = [1,1,1], k = 2

**Output:** 2

**Example 2:**

**Input:** nums = [1,2,3], k = 3

**Output:** 2

**Constraints:**

*   <code>1 <= nums.length <= 2 * 10<sup>4</sup></code>
*   `-1000 <= nums[i] <= 1000`
*   <code>-10<sup>7</sup> <= k <= 10<sup>7</sup></code>

## Solution

```kotlin
class Solution {
    fun subarraySum(nums: IntArray, k: Int): Int {
        var tempSum = 0
        var ret = 0
        val sumCount: MutableMap<Int, Int?> = HashMap()
        sumCount[0] = 1
        for (i in nums) {
            tempSum += i
            if (sumCount.containsKey(tempSum - k)) {
                ret += sumCount[tempSum - k]!!
            }
            if (sumCount[tempSum] != null) {
                sumCount[tempSum] = sumCount[tempSum]!! + 1
            } else {
                sumCount[tempSum] = 1
            }
        }
        return ret
    }
}
```