[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 581\. Shortest Unsorted Continuous Subarray

Medium

Given an integer array `nums`, you need to find one **continuous subarray** that if you only sort this subarray in ascending order, then the whole array will be sorted in ascending order.

Return _the shortest such subarray and output its length_.

**Example 1:**

**Input:** nums = [2,6,4,8,10,9,15]

**Output:** 5

**Explanation:** You need to sort [6, 4, 8, 10, 9] in ascending order to make the whole array sorted in ascending order.

**Example 2:**

**Input:** nums = [1,2,3,4]

**Output:** 0

**Example 3:**

**Input:** nums = [1]

**Output:** 0

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>4</sup></code>
*   <code>-10<sup>5</sup> <= nums[i] <= 10<sup>5</sup></code>

**Follow up:** Can you solve it in `O(n)` time complexity?

## Solution

```kotlin
class Solution {
    fun findUnsortedSubarray(nums: IntArray): Int {
        var end = -2
        var max = Int.MIN_VALUE
        for (i in nums.indices) {
            max = Math.max(max, nums[i])
            if (nums[i] < max) {
                end = i
            }
        }
        var start = -1
        var min = Int.MAX_VALUE
        for (i in nums.indices.reversed()) {
            min = Math.min(min, nums[i])
            if (nums[i] > min) {
                start = i
            }
        }
        return end - start + 1
    }
}
```