[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 213\. House Robber II

Medium

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are **arranged in a circle.** That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given an integer array `nums` representing the amount of money of each house, return _the maximum amount of money you can rob tonight **without alerting the police**_.

**Example 1:**

**Input:** nums = [2,3,2]

**Output:** 3

**Explanation:** You cannot rob house 1 (money = 2) and then rob house 3 (money = 2), because they are adjacent houses.

**Example 2:**

**Input:** nums = [1,2,3,1]

**Output:** 4

**Explanation:** Rob house 1 (money = 1) and then rob house 3 (money = 3). Total amount you can rob = 1 + 3 = 4.

**Example 3:**

**Input:** nums = [1,2,3]

**Output:** 3

**Constraints:**

*   `1 <= nums.length <= 100`
*   `0 <= nums[i] <= 1000`

## Solution

```kotlin
class Solution {
    fun rob(nums: IntArray): Int {
        val n = nums.size
        if (n == 0) {
            return 0
        }
        if (n == 1) {
            return nums[0]
        }
        if (n == 2) {
            return Math.max(nums[0], nums[1])
        }
        if (n == 3) {
            return Math.max(nums[0], Math.max(nums[1], nums[2]))
        }
        var max = Int.MIN_VALUE
        val inc = IntArray(n)
        val exc = IntArray(n)
        inc[0] = nums[0]
        exc[0] = 0
        inc[1] = nums[0]
        exc[1] = nums[1]
        inc[2] = nums[2] + nums[0]
        exc[2] = nums[2]
        for (i in 3 until n - 1) {
            inc[i] = Math.max(inc[i - 2], inc[i - 3]) + nums[i]
            exc[i] = Math.max(exc[i - 2], exc[i - 3]) + nums[i]
        }
        inc[n - 1] = inc[n - 2]
        exc[n - 1] = Math.max(exc[n - 3], exc[n - 4]) + nums[n - 1]
        for (i in 0 until n) {
            max = Math.max(max, Math.max(inc[i], exc[i]))
        }
        return max
    }
}
```