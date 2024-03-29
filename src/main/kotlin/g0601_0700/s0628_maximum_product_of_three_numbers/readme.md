[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 628\. Maximum Product of Three Numbers

Easy

Given an integer array `nums`, _find three numbers whose product is maximum and return the maximum product_.

**Example 1:**

**Input:** nums = [1,2,3]

**Output:** 6

**Example 2:**

**Input:** nums = [1,2,3,4]

**Output:** 24

**Example 3:**

**Input:** nums = [-1,-2,-3]

**Output:** -6

**Constraints:**

*   <code>3 <= nums.length <= 10<sup>4</sup></code>
*   `-1000 <= nums[i] <= 1000`

## Solution

```kotlin
class Solution {
    fun maximumProduct(nums: IntArray): Int {
        var min1 = Int.MAX_VALUE
        var min2 = Int.MAX_VALUE
        var max1 = Int.MIN_VALUE
        var max2 = Int.MIN_VALUE
        var max3 = Int.MIN_VALUE
        for (i in nums) {
            if (i > max1) {
                max3 = max2
                max2 = max1
                max1 = i
            } else if (i > max2) {
                max3 = max2
                max2 = i
            } else if (i > max3) {
                max3 = i
            }
            if (i < min1) {
                min2 = min1
                min1 = i
            } else if (i < min2) {
                min2 = i
            }
        }
        return Math.max(min1 * min2 * max1, max1 * max2 * max3)
    }
}
```