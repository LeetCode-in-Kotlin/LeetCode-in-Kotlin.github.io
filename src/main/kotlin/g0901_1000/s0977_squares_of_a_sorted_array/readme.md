[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 977\. Squares of a Sorted Array

Easy

Given an integer array `nums` sorted in **non-decreasing** order, return _an array of **the squares of each number** sorted in non-decreasing order_.

**Example 1:**

**Input:** nums = [-4,-1,0,3,10]

**Output:** [0,1,9,16,100]

**Explanation:** After squaring, the array becomes [16,1,0,9,100]. After sorting, it becomes [0,1,9,16,100].

**Example 2:**

**Input:** nums = [-7,-3,2,3,11]

**Output:** [4,9,9,49,121]

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>4</sup></code>
*   <code>-10<sup>4</sup> <= nums[i] <= 10<sup>4</sup></code>
*   `nums` is sorted in **non-decreasing** order.

**Follow up:** Squaring each element and sorting the new array is very trivial, could you find an `O(n)` solution using a different approach?

## Solution

```kotlin
import kotlin.math.abs

class Solution {
    fun sortedSquares(nums: IntArray): IntArray {
        var l = 0
        var r = nums.size - 1
        val res = IntArray(nums.size)
        // Iterate res from back to front. put the bigger of abs(l) * abs(l) and abs(r) * abs(r) at
        // res[i] and increment respectively
        for (i in nums.indices.reversed()) {
            // If the negative is larger, put it at the end and increment left ptr to next lower
            // negative
            if (abs(nums[l]) > nums[r]) {
                res[i] = nums[l] * nums[l]
                l++
            } else {
                res[i] = nums[r] * nums[r]
                r--
            }
        }
        return res
    }
}
```