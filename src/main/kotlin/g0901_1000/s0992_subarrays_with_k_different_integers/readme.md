[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 992\. Subarrays with K Different Integers

Hard

Given an integer array `nums` and an integer `k`, return _the number of **good subarrays** of_ `nums`.

A **good array** is an array where the number of different integers in that array is exactly `k`.

*   For example, `[1,2,3,1,2]` has `3` different integers: `1`, `2`, and `3`.

A **subarray** is a **contiguous** part of an array.

**Example 1:**

**Input:** nums = [1,2,1,2,3], k = 2

**Output:** 7

**Explanation:** Subarrays formed with exactly 2 different integers: [1,2], [2,1], [1,2], [2,3], [1,2,1], [2,1,2], [1,2,1,2]

**Example 2:**

**Input:** nums = [1,2,1,3,4], k = 3

**Output:** 3

**Explanation:** Subarrays formed with exactly 3 different integers: [1,2,1,3], [2,1,3], [1,3,4].

**Constraints:**

*   <code>1 <= nums.length <= 2 * 10<sup>4</sup></code>
*   `1 <= nums[i], k <= nums.length`

## Solution

```kotlin
class Solution {
    fun subarraysWithKDistinct(nums: IntArray, k: Int): Int {
        var res = 0
        var prefix = 0
        val cnt = IntArray(nums.size + 1)
        var i = 0
        var j = 0
        var uniqueCount = 0
        while (i < nums.size) {
            if (cnt[nums[i]]++ == 0) {
                uniqueCount++
            }
            if (uniqueCount > k) {
                --cnt[nums[j++]]
                prefix = 0
                uniqueCount--
            }
            while (cnt[nums[j]] > 1) {
                ++prefix
                --cnt[nums[j++]]
            }
            if (uniqueCount == k) {
                res += prefix + 1
            }
            i++
        }
        return res
    }
}
```