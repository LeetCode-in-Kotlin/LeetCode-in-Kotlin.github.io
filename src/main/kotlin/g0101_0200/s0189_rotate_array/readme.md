[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 189\. Rotate Array

Medium

Given an array, rotate the array to the right by `k` steps, where `k` is non-negative.

**Example 1:**

**Input:** nums = [1,2,3,4,5,6,7], k = 3

**Output:** [5,6,7,1,2,3,4]

**Explanation:** 

rotate 1 steps to the right: [7,1,2,3,4,5,6] 

rotate 2 steps to the right: [6,7,1,2,3,4,5] 

rotate 3 steps to the right: [5,6,7,1,2,3,4]

**Example 2:**

**Input:** nums = [-1,-100,3,99], k = 2

**Output:** [3,99,-1,-100]

**Explanation:** 

rotate 1 steps to the right: [99,-1,-100,3] 

rotate 2 steps to the right: [3,99,-1,-100]

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>-2<sup>31</sup> <= nums[i] <= 2<sup>31</sup> - 1</code>
*   <code>0 <= k <= 10<sup>5</sup></code>

**Follow up:**

*   Try to come up with as many solutions as you can. There are at least **three** different ways to solve this problem.
*   Could you do it in-place with `O(1)` extra space?

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    private fun reverse(nums: IntArray, l: Int, r: Int) {
        var l = l
        var r = r
        while (l <= r) {
            val temp = nums[l]
            nums[l] = nums[r]
            nums[r] = temp
            l++
            r--
        }
    }

    fun rotate(nums: IntArray, k: Int) {
        val n = nums.size
        val t = n - k % n
        reverse(nums, 0, t - 1)
        reverse(nums, t, n - 1)
        reverse(nums, 0, n - 1)
    }
}
```