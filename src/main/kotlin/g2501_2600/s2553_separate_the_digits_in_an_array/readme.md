[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2553\. Separate the Digits in an Array

Easy

Given an array of positive integers `nums`, return _an array_ `answer` _that consists of the digits of each integer in_ `nums` _after separating them in **the same order** they appear in_ `nums`.

To separate the digits of an integer is to get all the digits it has in the same order.

*   For example, for the integer `10921`, the separation of its digits is `[1,0,9,2,1]`.

**Example 1:**

**Input:** nums = [13,25,83,77]

**Output:** [1,3,2,5,8,3,7,7]

**Explanation:**

- The separation of 13 is [1,3].

- The separation of 25 is [2,5].

- The separation of 83 is [8,3].

- The separation of 77 is [7,7].

answer = [1,3,2,5,8,3,7,7]. Note that answer contains the separations in the same order.

**Example 2:**

**Input:** nums = [7,1,3,9]

**Output:** [7,1,3,9]

**Explanation:**

The separation of each integer in nums is itself. answer = [7,1,3,9].

**Constraints:**

*   `1 <= nums.length <= 1000`
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    fun separateDigits(nums: IntArray): IntArray {
        val arr = ArrayList<Int>()
        for (i in nums.indices.reversed()) {
            while (nums[i] > 0) {
                val r = nums[i] % 10
                arr.add(r)
                nums[i] = nums[i] / 10
            }
        }
        val num = IntArray(arr.size)
        var i = arr.size - 1
        for (j in arr) {
            num[i--] = j
        }
        return num
    }
}
```