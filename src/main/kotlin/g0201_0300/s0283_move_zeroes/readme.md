[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 283\. Move Zeroes

Easy

Given an integer array `nums`, move all `0`'s to the end of it while maintaining the relative order of the non-zero elements.

**Note** that you must do this in-place without making a copy of the array.

**Example 1:**

**Input:** nums = [0,1,0,3,12]

**Output:** [1,3,12,0,0]

**Example 2:**

**Input:** nums = [0]

**Output:** [0]

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>4</sup></code>
*   <code>-2<sup>31</sup> <= nums[i] <= 2<sup>31</sup> - 1</code>

**Follow up:** Could you minimize the total number of operations done?

## Solution

```kotlin
class Solution {
    fun moveZeroes(nums: IntArray) {
        var firstZero = 0
        for (i in nums.indices) {
            if (nums[i] != 0) {
                swap(firstZero, i, nums)
                firstZero++
            }
        }
    }

    private fun swap(index1: Int, index2: Int, numbers: IntArray) {
        val val2 = numbers[index2]
        numbers[index2] = numbers[index1]
        numbers[index1] = val2
    }
}
```