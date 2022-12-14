[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 260\. Single Number III

Medium

Given an integer array `nums`, in which exactly two elements appear only once and all the other elements appear exactly twice. Find the two elements that appear only once. You can return the answer in **any order**.

You must write an algorithm that runs in linear runtime complexity and uses only constant extra space.

**Example 1:**

**Input:** nums = [1,2,1,3,2,5]

**Output:** [3,5] **Explanation: ** [5, 3] is also a valid answer.

**Example 2:**

**Input:** nums = [-1,0]

**Output:** [-1,0]

**Example 3:**

**Input:** nums = [0,1]

**Output:** [1,0]

**Constraints:**

*   <code>2 <= nums.length <= 3 * 10<sup>4</sup></code>
*   <code>-2<sup>31</sup> <= nums[i] <= 2<sup>31</sup> - 1</code>
*   Each integer in `nums` will appear twice, only two integers will appear once.

## Solution

```kotlin
class Solution {
    fun singleNumber(nums: IntArray): IntArray {
        var xorSum = 0
        for (num in nums) {
            // will give xor of required nos
            xorSum = xorSum xor num
        }
        // find rightmost bit which is set
        val rightBit = xorSum and -xorSum
        var a = 0
        for (num in nums) {
            // xor only those number whose rightmost bit is set
            if (num and rightBit != 0) {
                a = a xor num
            }
        }
        return intArrayOf(a, a xor xorSum)
    }
}
```