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
import java.util.Arrays

class Solution {
    fun fourSum(nums: IntArray, target: Int): List<List<Int?>?> {
        var list: MutableList<List<Int?>?> = ArrayList()
        var j: Int
        var k: Int
        var l: Int
        Arrays.sort(nums)
        var i: Int = 0
        while (i < nums.size - 3) {
            if (i > 0 && nums[i] == nums[i - 1]) {
                i++
                continue
            }
            j = i + 1
            while (j < nums.size - 2) {
                if (j > i + 1 && nums[j] == nums[j - 1]) {
                    j++
                    continue
                }
                k = j + 1
                l = nums.size - 1
                while (k < l) {
                    val sum = nums[i] + nums[j] + nums[k] + nums[l]
                    if (sum == target) {
                        val l1 = ArrayList<Int?>()
                        l1.add(nums[i])
                        l1.add(nums[j])
                        l1.add(nums[k])
                        l1.add(nums[l])
                        list.add(l1)
                        l--
                        if (k < l && nums[l] == nums[l + 1]) {
                            l--
                        }
                    } else if (sum > target) {
                        l--
                    } else {
                        k++
                    }
                }
                j++
            }
            i++
        }
        list = ArrayList(LinkedHashSet(list))
        return list
    }
}
```