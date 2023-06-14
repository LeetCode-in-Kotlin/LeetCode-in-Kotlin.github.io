[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1546\. Maximum Number of Non-Overlapping Subarrays With Sum Equals Target

Medium

Given an array `nums` and an integer `target`, return _the maximum number of **non-empty** **non-overlapping** subarrays such that the sum of values in each subarray is equal to_ `target`.

**Example 1:**

**Input:** nums = [1,1,1,1,1], target = 2

**Output:** 2

**Explanation:** There are 2 non-overlapping subarrays [**1,1**,1,**1,1**] with sum equals to target(2).

**Example 2:**

**Input:** nums = [-1,3,5,1,4,2,-9], target = 6

**Output:** 2

**Explanation:** There are 3 subarrays with sum equal to 6. ([5,1], [4,2], [3,5,1,4,2,-9]) but only the first 2 are non-overlapping.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>-10<sup>4</sup> <= nums[i] <= 10<sup>4</sup></code>
*   <code>0 <= target <= 10<sup>6</sup></code>

## Solution

```kotlin
class Solution {
    fun maxNonOverlapping(nums: IntArray, target: Int): Int {
        var culSum = 0
        var res = 0
        val map: MutableMap<Int, Int> = HashMap()
        map[0] = 0
        for (num in nums) {
            culSum += num
            if (map.containsKey(culSum - target)) {
                res = Math.max(res, map[culSum - target]!! + 1)
            }
            map[culSum] = res
        }
        return res
    }
}
```