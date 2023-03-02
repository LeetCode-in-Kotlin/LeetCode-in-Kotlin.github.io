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
        val n = nums.size
        nums.sort()
        val result: MutableList<List<Int>> = ArrayList()
        for (i in 0 until n - 3) {
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue
            }
            if (nums[i].toLong() + nums[i + 1] + nums[i + 2] + nums[i + 3] > target) {
                break
            }
            if (nums[i].toLong() + nums[n - 3] + nums[n - 2] + nums[n - 1] < target) {
                continue
            }
            for (j in i + 1 until n - 2) {
                if (j > i + 1 && nums[j] == nums[j - 1]) {
                    continue
                }
                if (nums[j].toLong() + nums[j + 1] + nums[j + 2] > target - nums[i]) {
                    break
                }
                if (nums[j].toLong() + nums[n - 2] + nums[n - 1] < target - nums[i]) {
                    continue
                }
                val tempTarget = target - (nums[i] + nums[j])
                var low = j + 1
                var high = n - 1
                while (low < high) {
                    val curSum = nums[low] + nums[high]
                    if (curSum == tempTarget) {
                        val tempList: MutableList<Int> = ArrayList()
                        tempList.add(nums[i])
                        tempList.add(nums[j])
                        tempList.add(nums[low])
                        tempList.add(nums[high])
                        result.add(tempList)
                        low++
                        high--
                        while (low < high && nums[low] == nums[low - 1]) {
                            low++
                        }
                        while (low < high && nums[high] == nums[high + 1]) {
                            high--
                        }
                    } else if (curSum < tempTarget) {
                        low++
                    } else {
                        high--
                    }
                }
            }
        }
        return result
    }
}
```