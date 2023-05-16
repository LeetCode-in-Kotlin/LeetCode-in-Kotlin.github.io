[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1004\. Max Consecutive Ones III

Medium

Given a binary array `nums` and an integer `k`, return _the maximum number of consecutive_ `1`_'s in the array if you can flip at most_ `k` `0`'s.

**Example 1:**

**Input:** nums = [1,1,1,0,0,0,1,1,1,1,0], k = 2

**Output:** 6

**Explanation:** [1,1,1,0,0,<ins>**1**,1,1,1,1,**1**</ins>] Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.

**Example 2:**

**Input:** nums = [0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1], k = 3

**Output:** 10

**Explanation:** [0,0,<ins>1,1,**1**,**1**,1,1,1,**1**,1,1</ins>,0,0,0,1,1,1,1] Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   `nums[i]` is either `0` or `1`.
*   `0 <= k <= nums.length`

## Solution

```kotlin
class Solution {
    fun longestOnes(nums: IntArray, k: Int): Int {
        var onesCount = 0
        var max = 0
        var start = 0
        for (end in nums.indices) {
            if (nums[end] == 1) {
                onesCount++
            }
            if (end - start + 1 > onesCount + k) {
                if (nums[start] == 1) {
                    onesCount--
                }
                start++
            }
            max = max.coerceAtLeast(end - start + 1)
        }
        return max
    }
}
```