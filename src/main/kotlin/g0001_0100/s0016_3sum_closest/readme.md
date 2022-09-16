[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 16\. 3Sum Closest

Medium

Given an integer array `nums` of length `n` and an integer `target`, find three integers in `nums` such that the sum is closest to `target`.

Return _the sum of the three integers_.

You may assume that each input would have exactly one solution.

**Example 1:**

**Input:** nums = [-1,2,1,-4], target = 1

**Output:** 2

**Explanation:** The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).

**Example 2:**

**Input:** nums = [0,0,0], target = 1

**Output:** 0

**Constraints:**

*   `3 <= nums.length <= 1000`
*   `-1000 <= nums[i] <= 1000`
*   <code>-10<sup>4</sup> <= target <= 10<sup>4</sup></code>

## Solution

```kotlin
import java.util.Arrays

class Solution {
    fun threeSumClosest(nums: IntArray?, target: Int): Int {
        if (nums == null || nums.size < 3) {
            return 0
        }
        if (nums.size == 3) {
            return nums[0] + nums[1] + nums[2]
        }
        Arrays.sort(nums)
        val n = nums.size
        var sum = nums[0] + nums[1] + nums[2]
        for (i in 0 until n - 2) {
            if (nums[i] + nums[n - 1] + nums[n - 2] < target) {
                sum = nums[i] + nums[n - 1] + nums[n - 2]
                continue
            }
            if (nums[i] + nums[i + 1] + nums[i + 2] > target) {
                val temp = nums[i] + nums[i + 1] + nums[i + 2]
                return lessGap(sum, temp, target)
            }
            var j = i + 1
            var k = n - 1
            while (j < k) {
                val temp = nums[i] + nums[j] + nums[k]
                if (temp == target) {
                    return target
                }
                if (temp < target) {
                    j++
                } else {
                    k--
                }
                sum = lessGap(sum, temp, target)
            }
        }
        return sum
    }

    private fun lessGap(sum: Int, temp: Int, target: Int): Int {
        return if (Math.abs(sum - target) < Math.abs(temp - target)) sum else temp
    }
}
```