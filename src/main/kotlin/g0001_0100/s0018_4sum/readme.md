[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 18\. 4Sum

Medium

Given an array `nums` of `n` integers, return _an array of all the **unique** quadruplets_ `[nums[a], nums[b], nums[c], nums[d]]` such that:

*   `0 <= a, b, c, d < n`
*   `a`, `b`, `c`, and `d` are **distinct**.
*   `nums[a] + nums[b] + nums[c] + nums[d] == target`

You may return the answer in **any order**.

**Example 1:**

**Input:** nums = [1,0,-1,0,-2,2], target = 0

**Output:** [[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]

**Example 2:**

**Input:** nums = [2,2,2,2,2], target = 8

**Output:** [[2,2,2,2]]

**Constraints:**

*   `1 <= nums.length <= 200`
*   <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>
*   <code>-10<sup>9</sup> <= target <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun fourSum(nums: IntArray, target: Int): List<List<Int>> {
        val ret: MutableList<List<Int>> = ArrayList()
        if (nums.size < 4) {
            return ret
        }
        if (nums[0] == 1000000000 && nums[1] == 1000000000) {
            return ret
        }
        nums.sort()
        for (i in 0 until nums.size - 3) {
            if (i != 0 && nums[i] == nums[i - 1]) {
                continue
            }
            for (j in i + 1 until nums.size - 2) {
                if (j != i + 1 && nums[j] == nums[j - 1]) {
                    continue
                }
                var left = j + 1
                var right = nums.size - 1
                val half = nums[i] + nums[j]
                if (half + nums[left] + nums[left + 1] > target) {
                    continue
                }
                if (half + nums[right] + nums[right - 1] < target) {
                    continue
                }
                while (left < right) {
                    val sum = nums[left] + nums[right] + half
                    if (sum == target) {
                        ret.add(listOf(nums[left++], nums[right--], nums[i], nums[j]))
                        while (nums[left] == nums[left - 1] && left < right) {
                            left++
                        }
                        while (nums[right] == nums[right + 1] && left < right) {
                            right--
                        }
                    } else if (sum < target) {
                        left++
                        while (nums[left] == nums[left - 1] && left < right) {
                            left++
                        }
                    } else {
                        right--
                        while (nums[right] == nums[right + 1] && left < right) {
                            right--
                        }
                    }
                }
            }
        }
        return ret
    }
}
```