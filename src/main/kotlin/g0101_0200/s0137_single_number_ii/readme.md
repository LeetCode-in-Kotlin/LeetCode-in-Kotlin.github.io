[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 137\. Single Number II

Medium

Given an integer array `nums` where every element appears **three times** except for one, which appears **exactly once**. _Find the single element and return it_.

You must implement a solution with a linear runtime complexity and use only constant extra space.

**Example 1:**

**Input:** nums = [2,2,3,2]

**Output:** 3

**Example 2:**

**Input:** nums = [0,1,0,1,0,1,99]

**Output:** 99

**Constraints:**

*   <code>1 <= nums.length <= 3 * 10<sup>4</sup></code>
*   <code>-2<sup>31</sup> <= nums[i] <= 2<sup>31</sup> - 1</code>
*   Each element in `nums` appears exactly **three times** except for one element which appears **once**.

## Solution

```kotlin
class Solution {
    fun singleNumber(nums: IntArray): Int {
        var ones = 0
        var twos = 0
        for (num in nums) {
            ones = ones xor num and twos.inv()
            twos = twos xor num and ones.inv()
        }
        return ones
    }
}
```