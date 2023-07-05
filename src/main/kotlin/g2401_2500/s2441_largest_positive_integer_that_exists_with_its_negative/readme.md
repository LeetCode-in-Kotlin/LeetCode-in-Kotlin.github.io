[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2441\. Largest Positive Integer That Exists With Its Negative

Easy

Given an integer array `nums` that **does not contain** any zeros, find **the largest positive** integer `k` such that `-k` also exists in the array.

Return _the positive integer_ `k`. If there is no such integer, return `-1`.

**Example 1:**

**Input:** nums = [-1,2,-3,3]

**Output:** 3

**Explanation:** 3 is the only valid k we can find in the array.

**Example 2:**

**Input:** nums = [-1,10,6,7,-7,1]

**Output:** 7

**Explanation:** Both 1 and 7 have their corresponding negative values in the array. 7 has a larger value.

**Example 3:**

**Input:** nums = [-10,8,6,7,-2,-3]

**Output:** -1

**Explanation:** There is no a single valid k, we return -1.

**Constraints:**

*   `1 <= nums.length <= 1000`
*   `-1000 <= nums[i] <= 1000`
*   `nums[i] != 0`

## Solution

```kotlin
class Solution {
    fun findMaxK(nums: IntArray): Int {
        val arr = IntArray(nums.size)
        var j = 0
        for (i in nums.indices) {
            if (nums[i] < 0) {
                arr[j++] = nums[i]
            }
        }
        arr.sort()
        nums.sort()
        for (i in nums.indices) {
            for (num in nums) {
                if (arr[i] * -1 == num) {
                    return num
                }
            }
        }
        return -1
    }
}
```