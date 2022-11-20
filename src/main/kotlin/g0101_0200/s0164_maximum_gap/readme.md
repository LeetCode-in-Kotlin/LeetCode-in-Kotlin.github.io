[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 164\. Maximum Gap

Hard

Given an integer array `nums`, return _the maximum difference between two successive elements in its sorted form_. If the array contains less than two elements, return `0`.

You must write an algorithm that runs in linear time and uses linear extra space.

**Example 1:**

**Input:** nums = [3,6,9,1]

**Output:** 3

**Explanation:** The sorted form of the array is [1,3,6,9], either (3,6) or (6,9) has the maximum difference 3.

**Example 2:**

**Input:** nums = [10]

**Output:** 0

**Explanation:** The array contains less than 2 elements, therefore return 0.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>0 <= nums[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun maximumGap(nums: IntArray): Int {
        if (nums.size < 2) {
            return 0
        }
        var ret = Int.MIN_VALUE
        nums.sort()
        for (i in 0 until nums.size - 1) {
            if (nums[i + 1] - nums[i] > ret) {
                ret = nums[i + 1] - nums[i]
            }
        }
        return ret
    }
}
```