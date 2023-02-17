[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 665\. Non-decreasing Array

Medium

Given an array `nums` with `n` integers, your task is to check if it could become non-decreasing by modifying **at most one element**.

We define an array is non-decreasing if `nums[i] <= nums[i + 1]` holds for every `i` (**0-based**) such that (`0 <= i <= n - 2`).

**Example 1:**

**Input:** nums = [4,2,3]

**Output:** true

**Explanation:** You could modify the first 4 to 1 to get a non-decreasing array.

**Example 2:**

**Input:** nums = [4,2,1]

**Output:** false

**Explanation:** You cannot get a non-decreasing array by modifying at most one element.

**Constraints:**

*   `n == nums.length`
*   <code>1 <= n <= 10<sup>4</sup></code>
*   <code>-10<sup>5</sup> <= nums[i] <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    fun checkPossibility(nums: IntArray): Boolean {
        val n = nums.size
        if (n <= 2) {
            return true
        }

        var isModified = false
        for (i in 0..(n - 2)) {
            if (nums[i] <= nums[i + 1]) {
                continue
            }
            if (isModified) {
                return false
            }

            when {
                i == 0 || nums[i - 1] <= nums[i + 1] -> {
                    nums[i] = nums[i + 1]
                    isModified = true
                }
                i == n - 2 || nums[i] <= nums[i + 2] -> {
                    nums[i + 1] = nums[i]
                    isModified = true
                }
                else -> {
                    return false
                }
            }
        }
        return true
    }
}
```