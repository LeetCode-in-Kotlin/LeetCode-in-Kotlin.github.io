[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 169\. Majority Element

Easy

Given an array `nums` of size `n`, return _the majority element_.

The majority element is the element that appears more than `⌊n / 2⌋` times. You may assume that the majority element always exists in the array.

**Example 1:**

**Input:** nums = [3,2,3]

**Output:** 3

**Example 2:**

**Input:** nums = [2,2,1,1,1,2,2]

**Output:** 2

**Constraints:**

*   `n == nums.length`
*   <code>1 <= n <= 5 * 10<sup>4</sup></code>
*   <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>

**Follow-up:** Could you solve the problem in linear time and in `O(1)` space?

## Solution

```kotlin
class Solution {
    fun majorityElement(arr: IntArray): Int {
        var count = 1
        var majority = arr[0]
        // For Potential Majority Element
        for (i in 1 until arr.size) {
            if (arr[i] == majority) {
                count++
            } else {
                if (count > 1) {
                    count--
                } else {
                    majority = arr[i]
                }
            }
        }
        // For Confirmation
        count = 0
        for (j in arr) {
            if (j == majority) {
                count++
            }
        }
        return if (count >= arr.size / 2 + 1) {
            majority
        } else {
            -1
        }
    }
}
```