[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 15\. 3Sum

Medium

Given an integer array nums, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

Notice that the solution set must not contain duplicate triplets.

**Example 1:**

**Input:** nums = [-1,0,1,2,-1,-4]

**Output:** [[-1,-1,2],[-1,0,1]] 

**Example 2:**

**Input:** nums = []

**Output:** [] 

**Example 3:**

**Input:** nums = [0]

**Output:** [] 

**Constraints:**

*   `0 <= nums.length <= 3000`
*   <code>-10<sup>5</sup> <= nums[i] <= 10<sup>5</sup></code>

## Solution

```kotlin
import java.util.Arrays
import kotlin.collections.ArrayList

class Solution {
    fun threeSum(nums: IntArray): List<List<Int>> {
        Arrays.sort(nums)
        val len = nums.size
        val result: MutableList<List<Int>> = ArrayList()
        var l: Int
        var r: Int
        var i = 0
        while (i < len - 2) {
            l = i + 1
            r = len - 1
            while (r > l) {
                val sum = nums[i] + nums[l] + nums[r]
                if (sum < 0) {
                    l++
                } else if (sum > 0) {
                    r--
                } else {
                    val list: MutableList<Int> = ArrayList()
                    list.add(nums[i])
                    list.add(nums[l])
                    list.add(nums[r])
                    result.add(list)
                    while (l < r && nums[l + 1] == nums[l]) {
                        l++
                    }
                    while (r > l && nums[r - 1] == nums[r]) {
                        r--
                    }
                    l++
                    r--
                }
            }
            while (i < len - 1 && nums[i + 1] == nums[i]) {
                i++
            }
            i++
        }
        return result
    }
}
```