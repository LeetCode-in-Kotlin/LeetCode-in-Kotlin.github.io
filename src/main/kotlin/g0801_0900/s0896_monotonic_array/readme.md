[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 896\. Monotonic Array

Easy

An array is **monotonic** if it is either monotone increasing or monotone decreasing.

An array `nums` is monotone increasing if for all `i <= j`, `nums[i] <= nums[j]`. An array `nums` is monotone decreasing if for all `i <= j`, `nums[i] >= nums[j]`.

Given an integer array `nums`, return `true` _if the given array is monotonic, or_ `false` _otherwise_.

**Example 1:**

**Input:** nums = [1,2,2,3]

**Output:** true

**Example 2:**

**Input:** nums = [6,5,4,4]

**Output:** true

**Example 3:**

**Input:** nums = [1,3,2]

**Output:** false

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>-10<sup>5</sup> <= nums[i] <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    fun isMonotonic(nums: IntArray): Boolean {
        var i = 0
        while (i < nums.size - 1) {
            if (nums[i] > nums[i + 1]) {
                break
            }
            i++
        }
        if (i == nums.size - 1) {
            return true
        }
        i = 0
        while (i < nums.size - 1) {
            if (nums[i] < nums[i + 1]) {
                break
            }
            i++
        }
        return i == nums.size - 1
    }
}
```